# Project React Native Typescript - Update : 16 Févier 2021

## Initialisation d'un projet React Native Typescript

La commande suivante va nous permettre de créer une application react native (Il est préférable d'installer via npx et non en global)

```zsh
npx react-native <command>
```

Pour créer un projet sur la base de la commande précédente

```zsh
npx react-native init <MY_APP_NAME> --template react-native-template-typescript
```

Initialiser Git dans le répertoire de l'application

```zsh
git init
```

NOTE : Il se peut qu'il faille mettre à jour le fichier pod

Commenter le code dans le fichier ***podfile***. Pour inhiber le debugger Flipper

```java
# Note that if you have use_frameworks! enabled, Flipper will not work and
  # you should disable these next few lines.
  # use_flipper!
  # post_install do |installer|
  #   flipper_post_install(installer)
  # end
```

```zsh
npx pod-install
```
## Variables d'environnement

Les variables d'environnement servent à cacher les clés de l'application (api, secrets, ...) voir tous les éléments sensibles dans un fichier env.

```zsh
npm install react-native-config
```

### iOS

Nous utilisons cocoapods, executer le code suivante pour installer le package

```zsh
npx pod-install
```

### Android

Pour Android, ajouter manuellement les éléments suivants

android/settings.gradle

```java
+ include ':react-native-config'
+ project(':react-native-config').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-config/android')
android/app/build.gradle
```

android/app/build.gradle

```java
// 2nd line, add a new apply:
+ apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"

dependencies {
	implementation "com.facebook.react:react-native:+"  // From node_modules
+	implementation project(':react-native-config')
}
```

### Utilisation dans le code

Créer le fichier .env à la racine et copier :

```
API_URL=http://localhost:10000
```

```ts
import Config from 'react-native-config';

console.log(Config.API_URL);
```

## Lancer le programme
```zsh
react-native run-android
react-native run-ios
```


