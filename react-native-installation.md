# Update : 16 Févier 2021

## Installation de l'environnement de développement React Native

Ci-dessous, vous trouverez la procédure d'installation de React Native.

### Windows
Documentation : [https://facebook.github.io/react-native/docs/getting-started](https://facebook.github.io/react-native/docs/getting-started)

### macOS

Rendez-vous sur l'App Store pour télécharger ***Xcode***.
Vous devrez ensuite lancer le logiciel Xcode afin d'accepter les conditions générales d'utilisation.
Ensuite, allez dans l'onglet Locations des paramètres d'Xcode afin de vous assurer que le Command Line Tools n'est pas vide

:fire: IMPORTANT : Vous devez entrer toutes les commandes une par une.

Les commandes suivantes vont nous permettre de pouvoir installer des packages NPM globaux :

```zsh
chsh -s /bin/zsh
zsh
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo "export PATH=~/.npm-global/bin:\$PATH" >> ~/.zshrc
source ~/.zshrc
```

#### Installation des dépendances

Toute la procédure est disponible sur [ici](https://reactnative.dev/docs/environment-setup). Les codes suivants sont un résumé.

```zsh
brew install node
brew install watchman
```

La commande suivante nous permettre de compiler des applications ***iOS*** :

Ouvrir les préférences de XCode et sélectionner dans le champ ***Command line tool*** une version supérieur à 9.4

![image](https://reactnative.dev/assets/images/GettingStartedXcodeCommandLineTools-8259be8d3ab8575bec2b71988163c850.png)

```zsh
sudo gem install cocoapods
```

La commande suivante va nous permettre de compiler des applications ***Android*** :

```zsh
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
```

Les commandes suivantes vont nous permettre d'installer Expo :

```zsh
npm install -g expo-cli
```

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
