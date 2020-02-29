# Unity AndroidManifest Placeholders Resolver

## Description

You can add placeholders to your `AndroidManifest.xml` located under `Assets/Plugins/Android`.

ex:
```xml
<intent-filter ... >
    <data android:scheme="http" android:host="${hostName}" ... />
    ...
</intent-filter>
```

For more information about placeholders you can see the official documentation: https://developer.android.com/studio/build/manifest-build-variables

## Prerequisites

* Add to you `manifest.json` located under `Packages/` the following dependency.

```json
"com.martingonzalez.androidmanifestplaceholdersresolver": "git@github.com:MartinGonzalez/unity-android-manifest-placeholders-resolver.git",
```

* `Build System` must be set in gradle *(You can find this under BuildSettings)*

* In your `mainTemplate.gradle` located under `Assets/Plugins/Android` add the following placeholder.

```gradle
...

android {
    defaultConfig {     
        ##MANIFEST_PLACEHOLDERS##
    }

...
```
## Usage

* Go to your `AndroidManifest.xml` under `Assets/Plugins/Android` and add the placeholders you need. 

Example: I will add `"${hostName}"` and `"${scheme}"` placeholders.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest
    xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.unity3d.player"
    xmlns:tools="http://schemas.android.com/tools"
    android:installLocation="preferExternal">
    <supports-screens
        android:smallScreens="true"
        android:normalScreens="true"
        android:largeScreens="true"
        android:xlargeScreens="true"
        android:anyDensity="true"/>
    
    <application
        android:theme="@style/UnityThemeSelector"
        android:icon="@mipmap/app_icon"
        android:label="@string/app_name">
        <activity android:name="com.unity3d.player.UnityPlayerActivity"
                  android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            <meta-data android:name="unityplayer.UnityActivity" android:value="true" />

            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="http"
                      android:host="${hostName}"/>
            </intent-filter>
            
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="${scheme}"
                      android:host="gizmos" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

* Create a file called `GameAndroidManifestPlaceholders.xml` and place it anywhere under `Assets` folder.

*Important: XXXAndroidManifestPlaceholders.xml must be the naming of the file. ex: DeeplinkAndroidManifestPlaceholders.xml*

Inside this `xml` file you need to configure your placeholders and values.

Example:

```xml
<placeholders>
    <hostName>www.example.com</hostName>
    <scheme>mygame</scheme>
</placeholders>
```

* Build your app usign and you can inspect your patched AndroidManifest in your apk using `AndroidStudio`.

The result of the example would be

```xml
...
 <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="http"
          android:host="www.example.com" />
</intent-filter>

<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data android:scheme="mygame"
          android:host="gizmos" />
</intent-filter>
...
```

# Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. 

Please note we have a code of conduct, please follow it in all your interactions with the project.

## Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.