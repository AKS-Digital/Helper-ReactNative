#  Navigation - Update : 16 Février 2021

## Installation des dépendances

```zsh
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view @react-navigation/native
```

Ajouter ces dépendances pour utiliser la navigation par tab, drawer ou par stack

```zsh
npm install @react-navigation/stack @react-navigation/bottom-tabs
```

Sous macOS, pour les applications ios, utiliser cocoapods pour compléter l'installation

```zsh
npx pod-install
```

copier cette ligne en début du fichier ***src/App.tsx***

```ts
import 'react-native-gesture-handler';
```
## Flow de navigation

Dans une application, nous avons besoin d'un flow de navigation
- Authentification (Stack)
  - Connexion
  - Inscription
- Main (Bottom Tab)
  - Home
  - Profile
  - Settings

## Création du dossier ***navigation***

Créer le fichier ***src/navigation/navigator.tsx*** et copier :

```ts
import React from 'react';
import { SafeAreaView } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { AuthStack, MainStack } from './components';

interface Props {
  isAuthenticated: boolean;
}

export const Navigator = ({ isAuthenticated }: Props) => {
  return (
    <SafeAreaView style={{ flex: 1 }}>
      <NavigationContainer>
        {isAuthenticated ? <MainStack /> : <AuthStack />}
      </NavigationContainer>
    </SafeAreaView>
  );
};
```

Créer le fichier ***src/navigation/screens.ts*** et copier :

```ts
export const SCREENS = {
  SPLASH: 'Splash',
  SIGNIN: 'Sign In',
  SIGNUP: 'Sign Up',
  HOME: 'Home',
  PROFILE: 'Profile',
  SETTINGS: 'Settings',
};
```
Créer le fichier ***src/navigation/index.ts*** et copier :

```ts
export * from './Navigator';
export * from './screens';
```

Créer le fichier ***src/navigation/components/AuthStack.tsx*** et copier :

```ts
import React from 'react';
import { createStackNavigator } from '@react-navigation/stack';
import { SignInScreen, SignUpScreen } from '../../containers';
import { SCREENS } from '../screens';

const Stack = createStackNavigator();

export const AuthStack = () => {
  return (
    <Stack.Navigator initialRouteName={SCREENS.SIGNIN}>
      <Stack.Screen name={SCREENS.SIGNIN} component={SignInScreen} />
      <Stack.Screen name={SCREENS.SIGNUP} component={SignUpScreen} />
    </Stack.Navigator>
  );
};
```

Créer le fichier ***src/navigation/components/MainStack.tsx*** et copier :

```ts
import React from 'react';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import { HomeScreen, ProfileScreen, SettingsScreen } from '../../containers';
import { SCREENS } from '../screens';

const Tab = createBottomTabNavigator();

export const MainStack = () => {
  return (
    <Tab.Navigator initialRouteName={SCREENS.HOME}>
      <Tab.Screen name={SCREENS.HOME} component={HomeScreen} />
      <Tab.Screen name={SCREENS.PROFILE} component={ProfileScreen} />
      <Tab.Screen name={SCREENS.SETTINGS} component={SettingsScreen} />
    </Tab.Navigator>
  );
};
```

Créer le fichier ***src/navigation/components/index.ts*** et copier :

```ts
export * from './AuthStack';
export * from './MainStack';

