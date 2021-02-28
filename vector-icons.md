# Ajout d'icons dans le projet

```zs
npm install react-native-vector-icons @types/react-native-vector-icons
```

## iOS

Entrer la commande suivante pour mettre à jour le fichier pod

```zsh
npx react-native link react-native-vector-icons
npx pod-install
```

:fire: Attention: Si une erreur multiple command apparait. Aller dans Xcode ***build Phases*** et supprimer toutes les dépendances .ttf de ***Copy bundle Ressources***

## Android

Ajouter dans le fichier ***android/app/build.gradle*** les lignes suivantes

```
apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

Executer la commande suivante pour finaliser l'installation

```zsh
npx react-native run-android
```
