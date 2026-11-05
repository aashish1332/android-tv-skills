---
name: android-tv-memory
description: Optimize Android TV app memory - low-RAM targets, graphics/image handling, media buffers, bindings, foreground services, and profiling tools.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  topic: performance
  audience: developers
---

# Android TV Memory Optimization

Use this skill when optimizing memory for Android TV apps, especially media/content apps on low-RAM devices.

## When to use
- Profiling or reducing memory usage
- Targeting low-RAM TV devices
- Fixing OOM, jank, or LMK kills
- Optimizing images, media buffers, bindings

## Device categories
- Low-RAM: 1GB (720p UI/1080p video), 1.5GB (1080p UI/2160p video); isLowRamDevice()=true
- Regular: >=1.5GB/1080p, >=2GB/2160p
Always check `ActivityManager.isLowRamDevice()`.

## Memory targets (low-RAM, 1GB)
- Anonymous+Swap (Java+Native+Stack): <160 MB
- Graphics: 30-40 MB
- File: 60-80 MB
- Total (Anon+Swap+Graphics+File): <=280 MB peak
- Recommended Anon+Swap+Graphics <=200 MB

## Graphics memory
- Don't load images higher than device UI resolution (downscale 1080p→720p)
- Use hardware-backed bitmaps (Glide: enable Downsampler.ALLOW_HARDWARE_CONFIG)
- Avoid intermediate renders; use `forceHasOverlappingRendering:false`
- Use @color placeholders, not placeholder images
- Prefer standalone images over on-device composition
- Follow bitmap handling guide

## Anon+Swap
- Media buffers variable by RAM/resolution: 40-60MB (1GB/1080p), 60-80MB (1.5GB/1080p), 80-100MB (1.5GB/2160p), 100-120MB (2GB/2160p)
- Free media memory on episode change
- Release/stop media resources immediately on onStop
- Seeking: pre-buffer <=5s; buffer <=15-25MB; allocate/reuse seeking buffer
- Drop references when moving screens; avoid leaks (anonymous threads, unreleased video buffers)
- Don't hold bitmap references; no System.gc()
- Use LazyList/RecyclerView to reuse views
- Cache external content locally with update intervals
- Generate baseline profiles; shrink/obfuscate code
- No blocking network calls at startup

## Bindings
Bindings bring other apps into memory — release promptly.
- Play Integrity: check after loading screen, before playback; release StandardIntegrityManager
- Play Billing: init after loading screen; endConnection() before playback
- GMS FontsProvider: prefer standalone fonts on low-RAM
- Google Assistant library: replace if possible; use Gboard TTS for voice search

## Foreground services
- Audio apps: may keep running after user leaves; stop immediately after playback ends
- Other apps: must stop all foreground services when user leaves
- Background jobs: use WorkManager (constraints: network connected, storage not low, device idle)

## Jobs/Alarms
- Avoid AlarmManager (esp. set/setExact) on SDK 23+
- On low-RAM, run jobs only if necessary; WorkRequest only for updating recommendations after playback
- Job memory <30 MB

## Direct memory reclaim
Frequent direct reclaim = system memory pressure; reduce allocations.

## Tools
- Android Studio Memory Profiler (heap dump, native memory profiler)
- Android GPU Inspector (graphics allocations)
- Heap dump for leak debugging

## Reference
- https://developer.android.com/training/tv/playback/memory
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-memory\SKILL.md