# Ecrans d'onboarding

Pour créer notre page d'onboarding, nous allons utiliser le composant React Native (Flatlist) et ajouter une animation à la transition.

Le composant onboarding aura pour structure:
- le composant à afficher
- la pagination
- le bouton suivant ou ignorer

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

## Code Exemple

```ts
import React from 'react';
import {
  View,
  Text,
  StyleSheet,
  FlatList,
  useWindowDimensions,
  Animated,
} from 'react-native';
import Sign from '../assets/onboarding/sign.svg';
import Tracking from '../assets/onboarding/tracking.svg';
import SafePlace from '../assets/onboarding/safe_place.svg';
import CreditCard from '../assets/onboarding/credit_card.svg';
import { SvgProps } from 'react-native-svg';

interface Item {
  id: string;
  title: string;
  description: string;
  image: React.FC<SvgProps>;
}

const slides: Item[] = [
  {
    id: '1',
    title: 'Inscription Rapide & Facile',
    description:
      "Inscrivez vous afin de bénéficiez d'un suivi personnalisé de votre exposition aux différents polluants.",
    image: Sign,
  },
  {
    id: '2',
    title: 'Géolocalisation',
    description:
      'Utilisez la géolocalisation de votre téléphone pour calculer votre exposition en temps réel.',
    image: Tracking,
  },
  {
    id: '3',
    title: 'Zone verte',
    description:
      'Trouvez les zones vertes proche de vous où la polution est la moindre. Idéal pour vos balades et activités sportives',
    image: SafePlace,
  },
  {
    id: '4',
    title: 'Paiements rapides et faciles',
    description:
      'Débloquez les fonctionnalités les plus poussées pour une meilleure expérience utilisateur.',
    image: CreditCard,
  },
];

export const OnboardingScreen = () => {
  const [currentIndex, setCurrentIndex] = React.useState(0);
  const { width, height } = useWindowDimensions();
  const slidesRef = React.useRef(null);
  const scrollX = React.useRef(new Animated.Value(0)).current;

  const viewableItemsChanged = React.useRef(({ viewableItems }: any) =>
    setCurrentIndex(viewableItems[0].index),
  ).current;

  const viewConfig = React.useRef({ viewAreaCoveragePercentThreshold: 50 })
    .current;

  const RenderedItem = (item: Item) => (
    <View style={[styles.container, { width }]}>
      <item.image width={width * 0.6} height={height * 0.6} />
      <View style={{ flex: 1 }}>
        <Text style={styles.title}>{item.title}</Text>
        <Text style={styles.description}>{item.description}</Text>
      </View>
    </View>
  );

  const Pagination = ({ data, scrollX }: any) => (
    <View style={styles.pagination}>
      {data.map((_: any, i: any) => {
        const inputRange = [(i - 1) * width, i * width, (i + 1) * width];
        const dotWidth = scrollX.interpolate({
          inputRange,
          outputRange: [10, 20, 10],
          extrapolate: 'clamp',
        });
        const opacity = scrollX.interpolate({
          inputRange,
          outputRange: [0.3, 1, 0.3],
          extrapolate: 'clamp',
        });
        return (
          <Animated.View
            key={i}
            style={[styles.dot, { width: dotWidth, opacity }]}
          />
        );
      })}
    </View>
  );

  return (
    <View style={styles.container}>
      <FlatList
        ref={slidesRef}
        data={slides}
        horizontal
        showsHorizontalScrollIndicator={false}
        pagingEnabled
        bounces={false}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => <RenderedItem {...item} />}
        onScroll={Animated.event(
          [{ nativeEvent: { contentOffset: { x: scrollX } } }],
          {
            useNativeDriver: false,
          },
        )}
        scrollEventThrottle={32}
        onViewableItemsChanged={viewableItemsChanged}
        viewabilityConfig={viewConfig}
      />
      <Pagination data={slides} scrollX={scrollX} />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  title: {
    fontFamily: 'WorkSans-Bold',
    fontSize: 28,
    marginBottom: 10,
    color: '#493d8a',
    textAlign: 'center',
  },
  description: {
    fontFamily: 'WorkSans-Regular',
    color: '#62656b',
    textAlign: 'center',
    paddingHorizontal: 64,
  },
  pagination: {
    flexDirection: 'row',
    justifyContent: 'center',
    alignItems: 'center',
    height: 64,
  },
  dot: {
    height: 10,
    borderRadius: 5,
    marginHorizontal: 8,
    backgroundColor: '#493d8a',
  },
});
```

const { getDefaultConfig } = require('metro-config');
