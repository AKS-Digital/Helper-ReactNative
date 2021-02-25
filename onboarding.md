# Ecrans d'onboarding

Pour créer notre page d'onboarding, nous allons utiliser le composant React Native (Flatlist) et ajouter une animation à la transition.

Le composant onboarding aura pour structure:
- le composant à afficher
- la pagination
- le bouton suivant ou suivant

## pré-requis

Il faut avoir les packages suivants:

```zsh
npm install react-native-svg react-native-svg-transformer
```

dans le fichier ***metro.config.js*** ajouter:

```ts
/**
 * Metro configuration for React Native
 * https://github.com/facebook/react-native
 *
 * @format
 */

const { getDefaultConfig } = require('metro-config');

module.exports = (async () => {
  const {
    resolver: { sourceExts, assetExts },
  } = await getDefaultConfig();

  return {
    transformer: {
      getTransformOptions: async () => ({
        transform: {
          experimentalImportSupport: false,
          inlineRequires: false,
        },
      }),
      babelTransformerPath: require.resolve('react-native-svg-transformer'),
    },
    resolver: {
      assetExts: assetExts.filter((ext) => ext !== 'svg'),
      sourceExts: [...sourceExts, 'svg'],
    },
  };
})();
```

Créer un fichier ***declarations.d.ts*** à la racine du projet et copier 
```ts
declare module '*.svg' {
  import { SvgProps } from 'react-native-svg';
  const content: React.FC<SvgProps>;
  export default content;
}
```
