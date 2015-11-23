
# About me

* Shinobu Okano(@operandoOS)
* Mercari, Inc.
* ハンバーグ食べたい...

# まったりAndroid Framework Code Reading #2

* https://mandroidfcr.doorkeeper.jp/events/33925


# Meteoroid

* https://github.com/operando/Meteoroid

# Meteoroid

```java
File uploadFile = new File(..);

new Meteoroid.Builder()
    .token("your slack api token")
    .uploadFile(uploadFile)
    .channels("#general")
    .title("test titke")
    .initialComment("test comment")
    .build()
    .post(this);
```

# Meteoroid

```gradle
allprojects {
    repositories {
        jcenter()
        maven {
            url "https://dl.bintray.com/operandoos/maven/"
        }
    }
}

compile 'com.os.operando.meteoroid:meteoroid:1.0.1'
```

# Meteor

* https://github.com/operando/Meteor

# Meteor


# Meteorite

* https://github.com/operando/Meteorite

# Meteorite

```xml
// your app's AndroidManifest.xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
    package="com.os.operando.meteorite.sample"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <application
        android:name=".MyApplication"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">

        .....

        <meta-data
            android:name="com.os.operando.meteorite.slackToken"
            android:value="your Slack API token"/>
    </application>
</manifest>
```

# Meteorite

```xml
// your app's AndroidManifest.xml
<activity android:name="com.os.operando.meteorite.MeteoriteActivity"/>
<activity android:name="com.os.operando.meteorite.ScreenshotActivity" />

<receiver
    android:name="com.os.operando.meteor.MeteorReceiver"
    android:enabled="true"
    android:exported="true"/>
```


# Meteorite

```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Meteorite.init(this);
    }
}
```


# Meteoroid

```gradle
allprojects {
    repositories {
        jcenter()
        maven {
            url "https://dl.bintray.com/operandoos/maven/"
        }
    }
}

compile 'com.os.operando.meteorite:meteorite:1.0.1'
```

# Thanks!

* Thanks!