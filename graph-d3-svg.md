# Graphique avec d3 et react-native-svg - Update : 18 Février 2021

Nous allons créer un graphique en utilisant les bibliothèques svg et d3

## Installation des dépendances

```zsh
npm install d3 react-native-svg
```

### iOS

```zsh
npx pod-install
```

### Android

android/settings.gradle

```java
+ include ':react-native-svg'
+ project(':react-native-svg').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-svg/android')
```

android/app/build.gradle

```java
dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    //noinspection GradleDynamicVersion
    implementation "com.facebook.react:react-native:+"  // From node_modules
+    implementation project(':react-native-svg')
}
```
