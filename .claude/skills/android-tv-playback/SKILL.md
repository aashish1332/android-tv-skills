---
name: android-tv-playback
description: Android TV media playback - MediaSession, ExoPlayer, playback controls, ambient mode, Now Playing card, audio capabilities, and media memory handling.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  topic: playback
  audience: developers
---

# Android TV Playback

Use this skill when building media playback experiences on Android TV (video/audio apps).

## When to use
- Implementing video/audio playback
- Integrating MediaSession for system controls
- Handling ambient mode, Now Playing card
- Supporting multiple audio outputs/encodings
- Optimizing media memory

## MediaSession
Create session when preparing to play:
```kotlin
session = MediaSession(this, "MusicService").apply {
    setCallback(MediaSessionCallback())
    setFlags(MediaSession.FLAG_HANDLES_MEDIA_BUTTONS or MediaSession.FLAG_HANDLES_TRANSPORT_CONTROLS)
}
```
Activate when playback begins; request audio focus:
```kotlin
fun handlePlayRequest() {
    tryToGetAudioFocus()
    if (!session.isActive) session.isActive = true
}
```
Update PlaybackState (position, actions) and MediaMetadata (title, artist, artwork).

## Playback controls UX
| Button | Action |
|---|---|
| Center | Play/Pause |
| Right single press | Forward +N seconds |
| Left single press | Rewind -N seconds |
| Hold right | Scrub forward |
| Hold left | Scrub rewind |
| Up/Down | Peek info (no pause) |

## Ambient Mode
- Prevent entering ambient mode during user-initiated video playback via `FLAG_KEEP_SCREEN_ON`
- Audio apps should NOT prevent ambient mode; audio continues in ambient mode
- Partial wake lock keeps playback running; prevents energy saver
- Must comply with ambient mode quality guidelines for Play

```kotlin
activity?.window?.addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON)
// clear when done
activity?.window?.clearFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON)
```
Never set flag for auto-playing video/animations.

## Now Playing card
- Appears for active MediaSession with FLAG_HANDLES_TRANSPORT_CONTROLS
- Shows album art, title, app icon
- Deactivate session when playback fully stopped; pause 5-30 min delay
- Set session activity PendingIntent for launch target

## Audio capabilities
- Multiple outputs (TV speakers, HDMI, Bluetooth) with varying encodings
- Create AudioTrack with best supported AudioFormat
- Use getAudioProfiles()/getEncodings(); check isDirectPlaybackSupported()/getDirectPlaybackSupport()
- React to routing changes: OnRoutingChangedListener (API 24+), AudioDeviceCallback (API 23), HDMI plug broadcasts (21-22), Bluetooth broadcasts (<23)
- Recreate AudioTrack if format no longer supported; handle write errors (Error -32)

## Frame rate
Match content frame rate via frame rate API; adjust display settings.

## Audio description (Android 13+)
```kotlin
if (accessibilityManager.isAudioDescriptionRequested) { /* enable AD */ }
```
Monitor changes via AudioDescriptionRequestedChangeListener.

## Text scaling
Use wrap_content, sp units carefully, check FONT_SCALE.

## Memory for media
- Variable media buffers: 40-60MB (1GB/1080p), 60-80MB (1.5GB/1080p), 80-100MB (1.5GB/2160p), 100-120MB (2GB/2160p)
- Free media memory on episode change
- Release media resources immediately on onStop
- Seeking buffer <= 15-25MB, pre-buffer <= 5s until user selects position
- Use WorkManager for background jobs, not AlarmManager
- Foreground services: audio apps may keep running; all others must stop when user leaves

## Reference
- https://developer.android.com/training/tv/playback
- https://developer.android.com/training/tv/playback/media-session
- https://developer.android.com/training/tv/playback/controls
- https://developer.android.com/training/tv/playback/ambient-mode
- https://developer.android.com/training/tv/playback/now-playing
- https://developer.android.com/training/tv/playback/memory
- https://developer.android.com/training/tv/playback/audio-capabilities
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-playback\SKILL.md