---
name: android-tv-fundamentals
description: Android TV app fundamentals - project setup, manifest declarations, hardware handling, D-pad navigation, and controller management for leanback/TV apps.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  platform: android
  audience: developers
---

# Android TV App Fundamentals

Use this skill when creating or modifying an Android app that runs on Android TV / Google TV devices. Covers project setup, manifest, hardware, navigation, and controllers.

## When to use
- Starting a new TV app or adding TV support to an existing Android app
- Configuring AndroidManifest.xml for TV
- Handling TV hardware limitations
- Implementing D-pad navigation and controller support

## Project setup
- SDK tools 24.0.0+, platform Android 5.0 (API 21)+
- Single app supporting both mobile and TV preferred; use multiple APKs only if needed
- Compose for TV is the recommended UI toolkit (see `android-tv-compose`)

## Manifest requirements

Declare a TV launcher activity:
```xml
<activity
    android:name="com.example.TvActivity"
    android:label="@string/app_name"
    android:theme="@style/Theme.Leanback">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LEANBACK_LAUNCHER" />
    </intent-filter>
</activity>
```

Declare leanback feature (false if also mobile):
```xml
<uses-feature android:name="android.software.leanback" android:required="false" />
```

Touchscreen not required (mandatory for Play visibility):
```xml
<uses-feature android:name="android.hardware.touchscreen" android:required="false" />
```

Home screen banner + icon:
```xml
<application android:icon="@mipmap/ic_launcher" android:banner="@drawable/banner" ... />
```
Banner: 320x180 px, xhdpi, localized text.

Hardware features not available on TV — declare optional:
```xml
<uses-feature android:name="android.hardware.touchscreen" android:required="false"/>
<uses-feature android:name="android.hardware.faketouch" android:required="false"/>
<uses-feature android:name="android.hardware.telephony" android:required="false"/>
<uses-feature android:name="android.hardware.camera" android:required="false"/>
<uses-feature android:name="android.hardware.nfc" android:required="false"/>
<uses-feature android:name="android.hardware.location.gps" android:required="false"/>
<uses-feature android:name="android.hardware.microphone" android:required="false"/>
<uses-feature android:name="android.hardware.sensor" android:required="false"/>
<uses-feature android:name="android.hardware.wifi" android:required="false"/>
```

Also mark permission-implied features (RECORD_AUDIO→microphone, CAMERA→camera/autofocus, location, wifi) as not required. Note: permissions like `ACCESS_COARSE_LOCATION`/`ACCESS_FINE_LOCATION` are fine on API 21+.

Pointer remote support:
```xml
<application ...>
    <property android:name="android.software.leanback.supports_touch" android:value="true" />
</application>
```
Activity-level overrides application-level. Omitting defaults to false.

## Detect TV device
Kotlin:
```kotlin
val isTelevision = packageManager.hasSystemFeature(PackageManager.FEATURE_LEANBACK)
```

## Unsupported hardware
TVs lack: touchscreen, faketouch, telephony, camera, NFC, GPS, microphone (hardware), sensors, portrait screen. Some controllers have a mic — that's supported.

Check at runtime:
```kotlin
if (packageManager.hasSystemFeature(PackageManager.FEATURE_CAMERA)) { /* available */ }
```

## D-pad navigation
- D-pad is primary input; all controls must be reachable
- Framework auto-applies directional navigation; test thoroughly
- Override with `android:nextFocusUp/Down/Left/Right` when needed (loop to first)
- Provide clear focus indication (color/size/animation) via state-list drawables
- Never require touchscreen

## Back button
- Linear backward navigation only; no toggles
- Pressing back must eventually reach home screen
- No confirmation dialogs for exit; no virtual back button on screen
- Use OnBackPressedDispatcher for custom behavior

## Controllers
- D-pad minimum: up/down/left/right/select/back/home
- Handle disconnect/reconnect: configChanges="keyboard|keyboardHidden|navigation"
- Button mapping: KEYCODE_BUTTON_B/BACK→Back; SELECT/A/ENTER/DPAD_CENTER→Select; DPAD_*→Nav
- Media buttons map to MediaControllerCompat.TransportControls (play/pause/skip/ff/rewind)
- Don't override KEYCODE_MEDIA_PLAY/PAUSE when using MediaSession
- Multiple controllers: use getControllerNumber()
- Games: declare `android.hardware.gamepad` required="false"; don't depend on Start/Select/Menu

## Pointer remotes
Support click, hover, scroll, drag, and media scrubbing. Use Modifier.clickable()/hoverable() in Compose or state-list drawables with state_hovered in Views.

## Overscan
Maintain ~5% safe margin around layout edges.

## Low-power mode
Stop playback in onStop():
```kotlin
override fun onStop() { stopPlayback(); super.onStop() }
```

## Reference
- https://developer.android.com/training/tv/get-started
- https://developer.android.com/training/tv/get-started/hardware
- https://developer.android.com/training/tv/get-started/controllers
- https://developer.android.com/training/tv/get-started/navigation
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-fundamentals\SKILL.md