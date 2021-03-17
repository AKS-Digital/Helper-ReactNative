# Localisation en tâche de fond - Update : 16 Mars 2021

Cette partie est importante pour récupérer la position de l'utilisateur lorsque l'application est killé, en background ou en foreground

## Setup

### Android

Le ***Foreground service*** est une sorte de notification sur Android qui permet d'afficher à l'utilisateur que l'application utilise la géolocalisation en background.

#### Fichiers Native

Ajouter dans le fichier ***Android/app/src/main/AndroidManifest.xml*** les permissions et le foreground service

```xml
<manifest ...>
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
</manifest>
```

Ajouter dans le fichier ***Android/app/src/main/java/MainActivity.java***

```java
package com.cool.app;

import android.os.Bundle;
import com.facebook.react.ReactActivity;
// Add the following imports
import android.content.Intent;
import android.util.Log;
import com.facebook.react.bridge.WritableMap;
import com.facebook.react.bridge.Arguments;
import com.facebook.react.modules.core.DeviceEventManagerModule;

public class MainActivity extends ReactActivity {
  // ...

  // Add from here down to the end of your MainActivity
  public boolean isOnNewIntent = false;

  @Override
  public void onNewIntent(Intent intent) {
    super.onNewIntent(intent);
    isOnNewIntent = true;
    ForegroundEmitter();
  }

  @Override
  protected void onStart() {
    super.onStart();
    if(isOnNewIntent == true){}else {
        ForegroundEmitter();
    }
  }

  public  void  ForegroundEmitter(){
    // this method is to send back data from java to javascript so one can easily
    // know which button from notification or the notification button is clicked
    String  main = getIntent().getStringExtra("mainOnPress");
    String  btn = getIntent().getStringExtra("buttonOnPress");
    WritableMap  map = Arguments.createMap();
    if (main != null) {
        map.putString("main", main);
    }
    if (btn != null) {
        map.putString("button", btn);
    }
    try {
        getReactInstanceManager().getCurrentReactContext()
        .getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
        .emit("notificationClickHandle", map);
    } catch (Exception  e) {
    Log.e("SuperLog", "Caught Exception: " + e.getMessage());
    }
  }
}
```

Créer un fichier ***android/app/src/main/res/values/colors.xml*** et copier :

```xml
<resources>
  <item name="blue" type="color">#00C4D1</item>
  <integer-array name="androidcolors">
    <item>@color/blue</item>
  </integer-array>
</resources>
```
Il s'agit de la définition des couleurs dans le Manifest

## Installation des dépendances

```zsh
npm install 
```

