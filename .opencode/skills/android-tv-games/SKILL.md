---
name: android-tv-games
description: Build games for Android TV - landscape display, low latency mode, controller support, manifest, Play Games services, exit handling.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  topic: games
  audience: developers
---

# Android TV Games

Use this skill when building games for Android TV.

## When to use
- Creating a TV game
- Adding game controller support
- Integrating Google Play Games services
- Optimizing display/input for TV

## Display
- Always landscape; no portrait orientation
- Auto low latency mode (ALLM) for low latency: `Window.setPreferMinimalPostProcessing(true)` or attribute preferMinimalPostProcessing; check `Display.isMinimalPostProcessingSupported()`
- Game mode on displays

## Input
- No touch interface; design controls around D-pad
- Handle multiple controllers; track player-controller pairs
- Controller disconnect: pause game, show reconnect dialog with troubleshooting
- Keyboard layouts: use getKeyCodeForKeyLocation (Android 13+) for WASD etc.
- Don't depend on Start/Select/Menu buttons

## Manifest
```xml
<application android:isGame="true" ... />
<uses-feature android:name="android.hardware.gamepad" android:required="false" />
```
required="true" shows "Gamepad required" badge/warning but doesn't block install.

## Google Play Games services
- At least 5 earnable achievements
- Attempt sign-in on launch; stop asking after repeated declines
- Use Saved Games bound to Google Account (cross-device)
- Provide UI to delete local/cloud data (Settings)

## Exit
Provide obvious, D-pad-accessible exit UI element; don't rely on Home button.

## Web
No web browsing in TV games (WebView only for social logins).

## Networking
Check both WiFi and ethernet; no 3G/LTE check needed for TV-only apps.

## Controller best practices
- Communicate controller requirements in Play description
- Consistent button mapping (A=accept, B=cancel); allow remapping
- Query controller capabilities; fallback control schemes
- Use generic (non-branded) gamepad graphic for button mappings
- Show controller instructions using Android TV Gamepad Template

## Reference
- https://developer.android.com/training/tv/games
- https://developer.android.com/training/tv/get-started/controllers
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-games\SKILL.md