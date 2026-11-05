---
name: android-tv-quality
description: Android TV app quality - accessibility, publishing checklist, TV app quality guidelines, and Google Play distribution for TV.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  topic: quality
  audience: developers
---

# Android TV App Quality

Use this skill when preparing an Android TV app for release, improving accessibility, or meeting Play quality guidelines.

## When to use
- Pre-release checklist review
- Accessibility improvements
- Publishing to Google Play for TV
- Meeting TV app quality requirements

## Accessibility
- TalkBack compatibility; label all elements
- Respect text scaling: wrap_content dimensions, flexible layouts, sp units carefully
- Don't use sp for non-flexible components
- Check FONT_SCALE: `resources.configuration.fontScale`
- Audio description (Android 13+): `AccessibilityManager.isAudioDescriptionRequested()`, add listener
- Keyboard layouts: `getKeyCodeForKeyLocation()` (Android 13+)

## TV app quality guidelines
- https://developer.android.com/docs/quality-guidelines/tv-app-quality
- Ambient mode requirements
- Media playback quality (frictionless playback, direct-back for Live tab)
- Memory optimization requirements for low-RAM

## Publishing checklist
Form factor:
- LEANBACK_LAUNCHER activity declared
- Home screen banner 320x180 xhdpi per locale
- No unsupported hardware requirements declared
- Permissions don't imply unsupported hardware
- Ambient mode consideration for media apps

UI design:
- Landscape orientation specified
- Layout resources for landscape
- Text/controls visible from distance
- High-res bitmaps/icons for HDTV
- Overscan margins
- Every UI element works with D-pad and game controllers
- No touchscreen requirement
- Background image changes on browse

Search & discovery:
- Global search results provided
- TV-specific data fields
- Discovered content in details screen with immediate playback

Home screen:
- Meaningful channel names/icons
- Single content per program
- No ads in programs
- Correct metadata
- Play Next rules followed

Playback apps:
- MediaSession registered
- FLAG_KEEP_SCREEN_ON during video
- Ambient mode compliance
- Memory profiling + isLowRamDevice handling

Games:
- isGame flag set
- Controller support doesn't depend on Start/Select/Menu
- Generic gamepad graphic for mappings
- Ethernet + WiFi connectivity check
- Clean exit UI

## Distribution
- https://developer.android.com/training/tv/publishing/distribute
- Multiple APK support for mobile+TV
- Instant experiences (LEANBACK_LAUNCHER + LAUNCHER)

## Reference
- https://developer.android.com/training/tv/publishing/checklist
- https://developer.android.com/docs/quality-guidelines/tv-app-quality
- https://developer.android.com/training/tv/publishing/distribute
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-quality\SKILL.md