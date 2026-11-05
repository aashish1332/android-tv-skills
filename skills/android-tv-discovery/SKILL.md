---
name: android-tv-discovery
description: Android TV content discovery - home screen channels, programs, recommendations, Watch Next, global search, and preview videos.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  topic: discovery
  audience: developers
---

# Android TV Content Discovery

Use this skill when making app content discoverable on the Android TV home screen and search.

## When to use
- Adding recommendation channels/programs
- Implementing Watch Next
- Adding preview videos
- Integrating global search
- Migrating pre-Android 8 recommendations

## Home screen channels
- App should have at least one channel (first = default, auto-shown)
- Other channels require user approval to add
- Channel: meaningful name (not app name), icon, unique content
- Programs: single piece of content, suitable description, correct metadata, no ads/promos
- Preview images must fit available aspect ratios (no crop/stretch)
- Start playing program immediately on selection

## Recommendations compatibility
- Android 8.0+ (API 26+): channels API
- Before 8.0: single recommendations row
- Implement both; check SDK level:
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) { /* channels */ } else { /* row */ }
```

## Watch Next channel
System-maintained row ("Play Next"). Four types:
- WATCH_NEXT_TYPE_CONTINUE — stopped mid-content
- WATCH_NEXT_TYPE_NEXT — next episode available
- WATCH_NEXT_TYPE_NEW — new content following watched
- WATCH_NEXT_TYPE_WATCHLIST — saved by user

```kotlin
val builder = WatchNextProgram.Builder()
builder.setType(TvContractCompat.WatchNextPrograms.TYPE_MOVIE)
    .setWatchNextType(TvContractCompat.WatchNextPrograms.WATCH_NEXT_TYPE_CONTINUE)
    .setLastEngagementTimeUtcMillis(time)
    .setTitle("Title").setDescription("desc")
    .setPosterArtUri(uri).setIntentUri(uri)
    .setInternalProviderId(appProgramId)
val uri = context.contentResolver.insert(TvContractCompat.WatchNextPrograms.CONTENT_URI, builder.build().toContentValues())
```
Constraints: can't move/remove/hide row. Don't add clips or unrelated series.

## Preview videos
- No ads in previews
- 16:9 or 4:3 aspect ratio
- Publicly accessible: setPreviewVideoUri() with HTTPS URL (ExoPlayer-supported)
- DRM/unsupported: render on Surface via TvInputService (onSetSurface, onTune)
- Declare TvInputService in manifest with BIND_TV_INPUT permission

## Global search
Implement ContentProvider serving suggestions + searchable.xml.

Required columns: SUGGEST_COLUMN_TEXT_1, SUGGEST_COLUMN_PRODUCTION_YEAR, SUGGEST_COLUMN_DURATION.

searchable.xml:
```xml
<searchable xmlns:android="http://schemas.android.com/apk/res/android"
    android:label="@string/search_label"
    android:searchSuggestAuthority="com.example.provider"
    android:searchSuggestIntentAction="android.intent.action.VIEW"
    android:searchSuggestIntentData="content://com.example/video_db"
    android:searchSuggestSelection=" ?"
    android:includeInGlobalSearch="true" />
```

Manifest: provider exported="true"; activity handles ACTION_SEARCH with meta-data android.app.searchable.

Deep link: metadata matching (title/year/duration) yields deep link to app in details screen.

## Search from app
Query own content provider or internal catalog for in-app results.

## Guidelines
- https://developer.android.com/training/tv/discovery/guidelines-app-developers
- https://developer.android.com/training/tv/discovery/guidelines-tv-providers

## Reference
- https://developer.android.com/training/tv/discovery
- https://developer.android.com/training/tv/discovery/recommendations
- https://developer.android.com/training/tv/discovery/searchable
- https://developer.android.com/training/tv/discovery/preview-videos
- https://developer.android.com/training/tv/discovery/watch-next-add-programs
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-discovery\SKILL.md