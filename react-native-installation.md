# Installation de l'environnement de développement React Native - Update : 16 Févier 2021

Ci-dessous, vous trouverez la procédure d'installation de React Native.

## Windows
Documentation : [https://facebook.github.io/react-native/docs/getting-started](https://facebook.github.io/react-native/docs/getting-started)

## macOS

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
Les commandes suivantes vont nous permettre d'installer node et watchman pour react native

```zsh
brew install node
brew install watchman
```

Toute la procédure est disponible sur [ici](https://reactnative.dev/docs/environment-setup). Les codes suivants sont un résumé.


### iOS

Rendez-vous sur l'App Store pour télécharger ***Xcode***.
Vous devrez ensuite lancer le logiciel Xcode afin d'accepter les conditions générales d'utilisation.
Ensuite, allez dans l'onglet Locations des paramètres d'Xcode afin de vous assurer que le Command Line Tools n'est pas vide

Ouvrir les préférences de XCode et sélectionner dans le champ ***Command line tool*** une version supérieur à 9.4

![image](https://reactnative.dev/assets/images/GettingStartedXcodeCommandLineTools-8259be8d3ab8575bec2b71988163c850.png)

```zsh
sudo gem install cocoapods
```

### Android

La commande suivante va nous permettre de compiler des applications ***Android*** :

```zsh
brew install --cask adoptopenjdk/openjdk/adoptopenjdk8
```

Téléchargez puis lancez le logiciel [Android Studio](https://developer.android.com/studio/index.html) pour l'installer.

Lors de l'installation, choisissez l'option ***Custom*** puis cochez ***Android Virtual Device***. Laissez tous les autres choix par défaut puis terminez l'installation.
Lors de l'installation, vous pourriez rencontrer l'erreur suivante :

![](https://res.cloudinary.com/lereacteur-apollo/image/upload/v1574438985/10w-full-stack/React-Native-2019/system-blocked_f5ghbg.png)

Cliquez sur Open Security Preferences, puis sur le cadenas en bas à gauche, puis cliquez sur Allow pour autoriser le logiciel Intel (il s'agit de Intel HAXM, un logiciel qui améliorera les performances de la machine virtuelle Android).

![](https://res.cloudinary.com/lereacteur-apollo/image/upload/v1574439024/10w-full-stack/React-Native-2019/mac-security-allow-intel_kwoyip.png)

Lancez Android Studio et cliquez sur ***Configure*** puis sur ***SDK Manager***.

Cochez la case Show Package Details en bas à droite.
Dans la partie Android 9.0 (Pie), cochez Android SDK Platform 28, Intel x86 Atom_64 System Image et Google APIs Intel x86 Atom System Image.

![](https://res.cloudinary.com/lereacteur-apollo/image/upload/v1574439040/10w-full-stack/React-Native-2019/sdk_ycpheh.png)

Allez dans l'onglet SDK Tools et cochez la case Show Package Details.
Cochez 28.0.3 puis Apply ou OK.

![](https://res.cloudinary.com/lereacteur-apollo/image/upload/v1574439049/10w-full-stack/React-Native-2019/sdk-tools_hvdrsx.png)

Acceptez toutes les licenses en cliquant sur chaque élément de la liste de gauche puis Accept.
Android Studio lancera alors des téléchargements.

Les commandes suivantes vont nous permettre de pouvoir utiliser Android Studio depuis le Terminal :

```zsh
echo "export ANDROID_HOME=\$HOME/Library/Android/sdk" >> ~/.zshrc
echo "export PATH=\$PATH:\$ANDROID_HOME/emulator" >> ~/.zshrc
echo "export PATH=\$PATH:\$ANDROID_HOME/tools" >> ~/.zshrc
echo "export PATH=\$PATH:\$ANDROID_HOME/tools/bin" >> ~/.zshrc
echo "export PATH=\$PATH:\$ANDROID_HOME/platform-tools" >> ~/.zshrc
source ~/.zshrc
```

Accédez au menu principal de Android Studio et cliquez sur AVD Manager.

Cliquez sur Create Virtual Device.

Choisissez le Smartphone Pixel 3 XL.

Choisissez la version Pie 28 - x86 - Android 9.0 (Google APIs).


### Vérifications

Nous allons maintenant vérifier que tout fonctionne.

Le fichier .zshrc

Entrez la commande suivante pour lire le contenu du fichier .zshrc :

```zsh
cat ~/.zshrc
```

Vous devriez obtenir exactement ceci :

```zsh
export PATH=~/.npm-global/bin:$PATH
export ANDROID_HOME=$HOME/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Si vous n'obtenez pas le résultat attendu, vous devez modifier le fichier ***.zshrc*** manuellement avec Visual Studio Code.

Pour pouvoir ouvrir un fichier depuis le Terminal, ouvrez Visual Studio Code puis allez dans le menu ***View, Command Palette*** puis recherchez ***Shell command: Install 'code' command in PATH.***

Puis ouvrez le fichier ***.zshrc*** avec Visual Studio Code depuis le Terminal à l'aide de la commande suivante :

```zsh
code ~/.zshrc
```

Faites vos modifications, puis entrez la commande suivante pour que vos modifications soient prises en compte dans votre Terminal :

```zsh
source ~/.zshrc
``` 

Pour finir, entrer la commande suivante afin d'utiliser le debugger avec Android

```zsh
adb forward tcp:8080 tcp:8080
```

### Alternative

Les commandes suivantes vont nous permettre d'installer Expo (facultatif - uniquement pour les projets Expo)

```zsh
npm install -g expo-cli
```
