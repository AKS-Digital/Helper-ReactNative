# Intégration de logo à l'application

Nous allons intégrer des logos à l'application. Pour cela, créé votre ***logo en format PNG 1024x1024*** et se rendre sur le site [https://easyappicon.com/](https://easyappicon.com/)

Charger votre logo et régler le padding si nécessaire. Une fois fini, appuyer sur download et choisir ***iOS + Adaptative icon***

Un dossier sera généré.

## Android

Copier le contenu du dossier ***android*** dans le répertoire ***Android/app/src/main/res***

:fire: Attention : Supprimer tous les dossiers sauf le dossier values. Lors de la copie, fusionner les fichiers

## iOS

Copier le contenu du dossier ***ios/AppIcon.appiconset*** dans le répertoire ***ios/<NOM_DU_PROJET>/Images.xcassets/AppIcon.appiconset***

## Vérification

Vérifier que les logos sont bien utilisés en lancant les applications

```zsh
npx react-native run-android
npx react-native run-ios
```
