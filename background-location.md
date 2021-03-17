# Localisation en tâche de fond - Update : 16 Mars 2021

Cette partie est importante pour récupérer la position de l'utilisateur lorsque l'application est killé, en background ou en foreground

## Setup

### Android

Le ***Foreground service*** est une sorte de notification sur Android qui permet d'afficher à l'utilisateur que l'application utilise la géolocalisation en background.

#### Permissions

Ajouter dans le fichier ***Android/app/src/main/AndroidManifest.xml***

```java
<!-- Add the next two lines -->
  <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <application ...>
    // ...
    <meta-data
      android:name="com.supersami.foregroundservice.notification_channel_name"
      android:value="Sticky Title"
    />
    <meta-data
      android:name="com.supersami.foregroundservice.notification_channel_description"
      android:value="Sticky Description."
    />
    <meta-data
      android:name="com.supersami.foregroundservice.notification_color"
      android:resource="@color/blue"
    />
    <service android:name="com.supersami.foregroundservice.ForegroundService"></service>
    <service android:name="com.supersami.foregroundservice.ForegroundServiceTask"></service>
  </application>
```

## Installation des dépendances

```zsh
npm install 
```

