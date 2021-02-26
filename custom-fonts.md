# Import de custom fonts

## Récupération de la font

Se rendre sur le site [https://fonts.google.com/](https://fonts.google.com/) et télécharger l'ensemble de la font voulu ***(Download family)***.

## Renommage des fichiers

Le nommage des fonts Google sont normalement bien faites. Parfois il faut renommer les fichiers en utilisant le ***PostScript name***. Ceci est dans l'optique d'avoir une norme de nommage identique entre iOS et Android.

Exemple: 
- :ok_hand: WorkSans-Bold est correct
- :hankey: WorkSansBold n'est pas correct

Cette étape doit être faite si le nommage des fichiers n'est pas correct. utiliser des logiciels en ligne si besoin.

## Copier les fichiers dans le projet

Copier tous les fichiers dans le dossier ***src/assets/fonts/***

## Lier les fonts au projet

Pour react native > 0.60. Ajouter le code suivant dans le fichier ***react-native.config.js*** à la racine du projet

```js
module.exports = {
    "assets": [
        "fonts"
    ],
}
```

puis taper la ligne de commande suivante 

```zsh
npx react-native link
```

Cette commande aura pour impact :

Pour iOS: lier notre font au projet Xcode
Pour Android: copier la font dans le dossier ***android/app/src/main/assets/fonts***

## Création d'un thème

```ts
const theme = {
 title : {
   fontFamily: "WorkSans-Bold"
 },
 body : {
   fontFamily: "WorkSans-Regular"
 }
}
```

## Utilisation

Vous pouvez remarquer que le choix de ne pas utiliser la propriété fontWeight dans notre style. En effet, il existe d'autres solutions où vous pouvez l'utiliser mais cela ne fonctionne que sur iOS. Pour Android, vous devrez rédiger un service qui effectuera la traduction entre le fontWeight et le nom complet de votre police (ex: Sarpanch-ExtraBold pour fontWeight: 800).

```tsx
<Text style={theme.title}>My awesome font</Text>;

// or

<Text style={fontFamily:WorkSans-SemiBold}>My awesome font</Text>;
```
