# Intégration de logo à l'application

Nous allons intégrer des logos à l'application. Pour cela, récupérer votre logo en format png parfaitement carré en terme de pixel et se rendre sur le site [https://makeappicon.com/](https://makeappicon.com/)

Un dossier sera généré.

## Android

Copier le contenu du dossier ***android*** dans le répertoire ***Android/app/src/mùain/res***

## iOS

Copier le contenu du dossier ***ios/AppIcon.appiconset*** dans le répertoire ***ios/<NOM_DU_PROJET>/Images.xcassets/AppIcon.appiconset***

## Vérification

Vérifier que les logos sont bien utilisés en lancant les applications

```zsh
npx react-native run-android
npx react-native run-ios
```
