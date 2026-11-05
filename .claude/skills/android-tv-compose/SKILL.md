---
name: android-tv-compose
description: Build Android TV UIs with Jetpack Compose for TV - setup, TV Material components, focus management, scrollable layouts, catalog browser, and details screens.
license: MIT
compatibility: claude-code, opencode, codebuff, freebuff
metadata:
  category: android-tv
  framework: compose
  audience: developers
---

# Compose for TV

Use this skill when building Android TV user interfaces with Jetpack Compose for TV. Compose is the recommended way to build TV apps.

## When to use
- Setting up a new TV project with Compose
- Building catalog browsers, details screens, scrollable lists
- Migrating from Leanback to Compose for TV
- Handling focus and remote input in Compose

## Setup
Min: Android 5.0 (API 21). Requires Compose 1.3.0+, Kotlin 1.7.10+.

build.gradle.kts:
```kotlin
dependencies {
    val composeBom = platform("androidx.compose:compose-bom:2026.09.00")
    implementation(composeBom)
    implementation("androidx.activity:activity-compose:1.13.0")
    implementation("androidx.compose.ui:ui-tooling-preview")
    debugImplementation("androidx.compose.ui:ui-tooling")
    implementation("androidx.tv:tv-material:1.0.0")
}
```

Use `androidx.tv.material3` components (TV-optimized: clear focus indicators, remote-friendly input). Mixing mobile Compose Material with TV Material can cause inconsistent theming — prefer TV versions.

## Key differences from mobile Compose
- TV Material components designed for 10-foot viewing
- Each library has its own MaterialTheme — don't mix
- Components: Buttons, Cards, Carousel, ImmersiveList, Lists, NavigationDrawer, Tabs

## Focus management
- Use `FocusRequester` to control initial/programmatic focus
- `Modifier.clickable()` for click/selection (matches D-pad center)
- `Modifier.hoverable()` + InteractionSource for hover states (pointer remotes)
- Standard scrollable containers support pointer/touch scrolling by default

## Catalog browser
- Use LazyRow/LazyColumn with item cards
- Background image changes as user browses content
- Horizontal rows of content, vertical categories

## Details screen
- Large imagery, title, description, action buttons (Play, Add to Watchlist)
- Clear focus order from top to actions

## Scrollable layouts
- Use LazyList (LazyRow/LazyColumn) to reuse views
- Avoid recreating list elements

## Overscan
Maintain safe margins ~5% around edges.

## Large text/controls
Ensure legibility from 10 feet; use wrap_content appropriately.

## Navigation
- Combine with Navigation component for back stack
- Deep linking support

## Samples
- TV Material Catalog: https://github.com/android/tv-samples/tree/main/TvMaterialCatalog
- JetStream: https://github.com/android/tv-samples/tree/main/JetStreamCompose
- Codelab: https://developer.android.com/codelabs/compose-for-tv-introduction

## Reference
- https://developer.android.com/training/tv/playback/compose
- https://developer.android.com/training/tv/playback/compose/browse
- https://developer.android.com/training/tv/playback/compose/details
- https://developer.android.com/training/tv/playback/compose/lists
path: C:\Users\HP\AppData\Local\Temp\opencode\android-tv-skills\skills\android-tv-compose\SKILL.md