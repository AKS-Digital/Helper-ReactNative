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
```
## Variables d'environnement

Les variables d'environnement servent à cacher les clés de l'application (api, secrets, ...) voir tous les éléments sensibles dans un fichier env.

```zsh
npm install react-native-config
```

### iOS

Nous utilisons cocoapods, executer le code suivante pour installer le package

```zsh
npx pod-install
```

### Android

Pour Android, ajouter manuellement les éléments suivants

android/settings.gradle

```java
+ include ':react-native-config'
+ project(':react-native-config').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-config/android')
```

android/app/build.gradle

```java
// 2nd line, add a new apply:
+ apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"

dependencies {
	implementation "com.facebook.react:react-native:+"  // From node_modules
+	implementation project(':react-native-config')
}
```

### Utilisation dans le code

Créer le fichier .env à la racine et copier :

```
API_URL=http://localhost:10000
```

```ts
import Config from 'react-native-config';

console.log(Config.API_URL);
```

## Autorisation du traffic internet

### Android

Nous avons besoin de définir le serveur en localhost. pour cela dans le fichier ***AndroidManifest.xml*** ajouter ces lignes:

```xml
<manifest ...>
  <uses-permission android:name="android.permission.INTERNET" />
  <application
    android:name=".MainApplication"
+   android:usesCleartextTraffic="true"
+   android:networkSecurityConfig="@xml/network_security_config"
  ...>
    <activity android:name=".MainActivity" ...>
      ...
    </activity>
  </application>
</manifest>
```

Nous souhaitons uniquement autoriser quelques adresses. Créer le fichier ***network_security_config.xml*** dans le répertoire ***app/src/main/res/xml*** et copier

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
  <domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="false">localhost</domain>
    <domain includeSubdomains="false">10.0.2.2</domain>
    <base-config cleartextTrafficPermitted="true"/>
  </domain-config>
</network-security-config>
```

Pour ajouter une adresse ip en particulier:

```xml
    <domain includeSubdomains="false"><CUSTOM_IP></domain>
```

### iOS

Pas besoin d'ajouter d'exception dans React mais vous pouvez contrôler dans le fichier ***info.plist*** si l'élement suivant est défini:

```xml
<key>NSAppTransportSecurity</key>
<dict>
<key>NSExceptionDomains</key>
  <dict>
  <key>localhost</key>
  <dict>
    <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
    <true/>
  </dict>
  </dict>
</dict>
```


## Lancer le programme
```zsh
react-native run-android
react-native run-ios
```


