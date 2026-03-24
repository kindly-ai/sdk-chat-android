# Configuration

Enable crash reporting

```Kotlin
KindlySDK.enableCrashReporting = true
```

Sentry is bundled with the SDK and used automatically for crash reporting when enabled.

Allow attachments from camera

```Kotlin
KindlySDK.config.allowCameraAccess = true
```

Allow attachments from photo library

```Kotlin
KindlySDK.config.allowPhotoLibraryAccess = true
```

## Status Bar Color Matching

Control how the SDK handles the device's status bar color to match the chat interface.

### Auto-match Status Bar Color

Automatically match the status bar color to the SDK header color (enabled by default):

```Kotlin
KindlySDK.config.autoMatchStatusBarColor = true
```

When enabled, the SDK will:
- Detect if the host app is using a system default status bar color
- If yes, update the status bar to match the SDK header color
- If the host app has a custom status bar color, respect it and leave it unchanged

### Force Override Status Bar Color

Force the SDK to always override the status bar color, regardless of the host app's settings:

```Kotlin
KindlySDK.config.forceOverrideStatusBarColor = true
```

When enabled, the SDK will always set the status bar color to match the header, even if the host app has a custom color.

**Note:** This setting only takes effect when `autoMatchStatusBarColor` is also enabled.

### Usage Examples

**Default behavior (respectful mode):**
```Kotlin
KindlySDK.config.autoMatchStatusBarColor = true
KindlySDK.config.forceOverrideStatusBarColor = false  // default
```

**Always override host app's status bar:**
```Kotlin
KindlySDK.config.autoMatchStatusBarColor = true
KindlySDK.config.forceOverrideStatusBarColor = true
```

**Disable status bar color matching:**
```Kotlin
KindlySDK.config.autoMatchStatusBarColor = false
```
