# Internationalization (traduction) - Update : 18 Février 2021

L'Internationalization vous permet de localiser votre application, en personnalisant l'expérience pour des régions, des langues ou des cultures spécifiques

## Installation des dépendances

```zsh
npm install react-native-localize
```

## Création du context et du hooks

Créer le fichier ***src/providers/LocaleProvider.tsx*** et copier :

```tsx
import React from 'react';
import * as RNLocalize from 'react-native-localize';

export const LocaleStateContext = React.createContext('');
export const LocaleDispatchContext = React.createContext<
  React.Dispatch<string>
>(() => null);

interface Props {
  children: React.ReactNode;
}

export const LocaleProvider = React.memo(({ children }: Props) => {
  const [locale, setLocale] = React.useState('');

  React.useEffect(() => {
    //  { countryCode: "US", languageTag: "en-US", languageCode: "en", isRTL: false },
    setLocale(RNLocalize.getLocales()[0].languageTag);
  }, []);

  return (
    <LocaleDispatchContext.Provider value={setLocale}>
      <LocaleStateContext.Provider value={locale}>
        {children}
      </LocaleStateContext.Provider>
    </LocaleDispatchContext.Provider>
  );
});
```

Créer le fichier ***src/hooks/useTranslation.ts*** et copier :

```ts
import React from 'react';
import { LocaleStateContext, LocaleDispatchContext } from '../providers';

export const useTranslation = () => {
  const locale = React.useContext(LocaleStateContext);
  const setLocale = React.useContext(LocaleDispatchContext);

  return { locale, setLocale };
};
```

## Utilisation

Fournir le provider à l'application dans le fichier ***src/App.tsx***

```tsx
import { LocaleProvider } from './providers';

export default function App() {
  return (
    <LocaleProvider>
      <StoreProvider>
        <Navigator />
      </StoreProvider>
    </LocaleProvider>
  );
}
```

Dans un composant en l'utilisant en tant que hooks

```tsx
import { useTranslation } from '../hooks';

// Dans le composant React
const { locale, setLocale } = useTranslation();
console.log(locale);
```


