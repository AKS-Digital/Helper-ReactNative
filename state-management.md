# State Management - Update : 17 Février 2021

Le state management utilise les principes de React que sont le useContext et le useReducer. Nous allons rendre le tout persistant en utilisant AsyncStorage

## Installation  des dépendances

```zsh
npm install @react-native-async-storage/async-storage
```

## Création des fichiers

Créer le fichier ***src/store/index.ts*** et copier :

```ts
export * from './initialState';
export * from './storeReducer';
export * from './StoreProvider';
```

Créer le fichier ***src/store/initialState.ts*** et copier :

```ts
export interface IStoreState {
  isStateLoaded: boolean;
}

export interface IStoreDispatch {
  type: null | 'LOAD_STORAGE_STATE';
  payload: any;
}

export const initialState: IStoreState = {
  isStateLoaded: false,
};
```

Créer le fichier ***src/store/storeReducer.ts*** et copier :

```ts
import { IStoreState, IStoreDispatch } from './initialState';

export const storeReducer = (
  state: IStoreState,
  { type, payload }: IStoreDispatch,
) => {
  switch (type) {
    case 'LOAD_STORAGE_STATE':
      return {
        ...state,
        isStateLoaded: payload,
      };

    default: {
      throw new Error(`auth state error ${type}`);
    }
  }
};
```

Créer le fichier ***src/store/StoreProvider.tsx*** et copier :

```ts
import React from 'react';
import { storeReducer } from './storeReducer';
import { initialState, IStoreDispatch } from './initialState';

interface Props {
  children: React.ReactNode;
}

export const StoreStateContext = React.createContext(initialState);
export const StoreDispatchContext = React.createContext<
  React.Dispatch<IStoreDispatch>
>(() => null);

export const StoreProvider = React.memo(({ children }: Props) => {
  const [store, dispatch] = React.useReducer(storeReducer, initialState);

  return (
    <StoreDispatchContext.Provider value={dispatch}>
      <StoreStateContext.Provider value={store}>
        {children}
      </StoreStateContext.Provider>
    </StoreDispatchContext.Provider>
  );
});
```

Utilisation du storeProvider dans le fichier ***App.tsx***

```ts
//...
import { StoreProvider } from './store';
//...

<StoreProvider>
  //... Application
</StoreProvider>
```

Créer le custom hooks useStore dans ***src/hooks/useStore.ts***

```ts
import React from 'react';
import { StoreStateContext, StoreDispatchContext } from '../store';

export const useStore = () => {
  const store = React.useContext(StoreStateContext);
  const dispatch = React.useContext(StoreDispatchContext);

  const loadStorage = (isStateLoaded: boolean) => {
    dispatch({ type: 'LOAD_STORAGE_STATE', payload: isStateLoaded });
  };

  return { store, loadStorage };
};
```
