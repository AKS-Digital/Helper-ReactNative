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

## Lancer le programme
```zsh
react-native run-android
react-native run-ios
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

