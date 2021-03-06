# Authentification - Update : 4 Mars 2021

L'authentification est une partie importante d'une application React Native dans la gestion des accès à la base de données coté serveur

## Pré-requis

```zsh
npm install axios @react-native-async-storage/async-storage react-native-config
```

## Création de l'objet d'état

Créer un fichier ***src/auth/initialState.ts*** et copier

```ts
export interface IApiState {
  accessToken: string | null;
  refreshToken: string | null;
  isRestored: boolean;
  isAuthenticated: boolean;
}

export const initialState: IApiState = {
  accessToken: null,
  refreshToken: null,
  isRestored: false,
  isAuthenticated: false,
};
```

## Création du reducer d'état

Créer un fichier ***src/auth/apiReducer.ts*** et copier

```ts
import { IApiState } from './initialState';

export interface IDispatch {
  type: 'LOAD_STORAGE_DATA' | 'LOGIN' | 'LOGOUT';
  payload: any;
}

export const apiReducer = (state: IApiState, { type, payload }: IDispatch) => {
  switch (type) {
    case 'LOAD_STORAGE_DATA':
      return { ...state, ...payload };
    case 'LOGIN':
      return {
        ...state,
        accessToken: payload.accessToken,
        refreshToken: payload.refreshToken,
        isAuthenticated: true,
      };
    case 'LOGOUT':
      return {
        ...state,
        accessToken: null,
        refreshToken: null,
        isAuthenticated: false,
      };
    default: {
      throw new Error(`state error ${type}`);
    }
  }
};
```

## Création du provider d'authentification

Créer un fichier ***src/auth/ApiProvier.tsx*** et copier

```tsx
import React from 'react';
import AsyncStorage from '@react-native-async-storage/async-storage';
import axios from 'axios';
import { initialState } from './initialState';
import { apiReducer, IDispatch } from './apiReducer';

interface Props {
  children?: React.ReactNode;
}

export const ApiStateContext = React.createContext(initialState);
export const ApiDispatchContext = React.createContext<
  React.Dispatch<IDispatch>
>(() => null);

export const ApiProvider = ({ children }: Props) => {
  const isMounted = React.useRef(false);
  const [state, dispatch] = React.useReducer(apiReducer, initialState);

  // Persist auth state
  React.useEffect(() => {
    (async () => {
      if (isMounted.current === false) {
        // get data
        try {
          const jsonValue = await AsyncStorage.getItem('@api');
          if (jsonValue) {
            const { accessToken, refreshToken } = JSON.parse(jsonValue);
            dispatch({
              type: 'LOAD_STORAGE_DATA',
              payload: { accessToken, refreshToken, isRestored: true },
            });
          }
        } catch (e) {
          console.log(e.message);
        } finally {
          isMounted.current = true;
        }
      } else {
        // persist data
        try {
          const jsonValue = JSON.stringify(state);
          await AsyncStorage.setItem('@api', jsonValue);
        } catch (e) {
          console.log(e.message);
        }
      }
    })();
  }, [state]);

  // Silently refresh tokens
  React.useEffect(() => {
    if (state.isRestored && state.refreshToken) {
      axios
        .post('http://localhost:3000/auth/refresh-token', {
          refreshToken: state.refreshToken,
        })
        .then((response) => {
          if (response) {
            dispatch({
              type: 'LOGIN',
              payload: { ...response.data },
            });
          }
        })
        .catch(() =>
          dispatch({
            type: 'LOGOUT',
            payload: null,
          }),
        );
    }
  }, [state.isRestored]);

  return (
    <ApiStateContext.Provider value={state}>
      <ApiDispatchContext.Provider value={dispatch}>
        {children}
      </ApiDispatchContext.Provider>
    </ApiStateContext.Provider>
  );
};
```

## Création du custom hooks useApi

Créer un fichier ***src/auth/useApi.tsx*** et copier

```tsx
import React from 'react';
import { ApiStateContext, ApiDispatchContext } from './ApiProvider';
import axios from 'axios';

export const useApi = () => {
  const [isLoading, setIsLoading] = React.useState(false);
  const apiState = React.useContext(ApiStateContext);
  const dispatch = React.useContext(ApiDispatchContext);

  const api = axios.create({
    baseURL: 'http://localhost:3000/',
    timeout: 1000,
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${apiState.accessToken}`,
    },
  });

  const get = async (endpoint: string, query?: object) => {
    setIsLoading(true);
    try {
      const { data } = await api.get(endpoint, { params: query });
      return data;
    } catch (err) {
      try {
        const tokens = await refreshCredentials();
        if (tokens) {
          const response = await api.get(endpoint, {
            headers: {
              Authorization: `Bearer ${tokens.data.accessToken}`,
            },
            params: query,
          });
          dispatch({
            type: 'LOGIN',
            payload: { ...tokens.data },
          });
          return response.data;
        }
        return null;
      } catch (err) {
        return null;
      }
    } finally {
      setIsLoading(false);
    }
  };
  const post = async (endpoint: string, body?: object) => {
    setIsLoading(true);
    try {
      const { data } = await api.post(endpoint, body);
      return data;
    } catch (err) {
      try {
        const tokens = await refreshCredentials();
        if (tokens) {
          const response = await api.post(endpoint, body, {
            headers: {
              Authorization: `Bearer ${tokens.data.accessToken}`,
            },
          });
          dispatch({
            type: 'LOGIN',
            payload: { ...tokens.data },
          });
          return response.data;
        }
        return null;
      } catch (err) {
        return null;
      }
    } finally {
      setIsLoading(false);
    }
  };
  const login = async (email: string, password: string) => {
    try {
      setIsLoading(true);
      const response: any = await api.post('/auth/login', { email, password });
      if (response) {
        dispatch({
          type: 'LOGIN',
          payload: { ...response.data },
        });
      }
    } catch (err) {
      console.log(err.message);
    } finally {
      setIsLoading(false);
    }
  };
  const logout = async () => {
    try {
      setIsLoading(true);
      await api.delete('/auth/logout', {
        data: {
          refreshToken: apiState.refreshToken,
        },
      });
    } catch (err) {
      console.log(err.message);
    } finally {
      dispatch({
        type: 'LOGOUT',
        payload: null,
      });
      setIsLoading(false);
    }
  };
  const refreshCredentials = async () => {
    try {
      setIsLoading(true);
      const response = await api.post('/auth/refresh-token', {
        refreshToken: apiState.refreshToken,
      });
      if (response) {
        dispatch({
          type: 'LOGIN',
          payload: { ...response.data },
        });
      }
      return response;
    } catch (err) {
      dispatch({
        type: 'LOGOUT',
        payload: null,
      });
    } finally {
      setIsLoading(false);
    }
  };

  return { isLoading, apiState, login, logout, get, post };
};
```
