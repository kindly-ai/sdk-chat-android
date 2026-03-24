# Initialization

## Prerequisites

The Kindly SDK uses `java.time` APIs internally. To support devices running Android API < 26, your app must enable core library desugaring.

Add the following to your app-level `build.gradle`:

```gradle
android {
    compileOptions {
        coreLibraryDesugaringEnabled true
    }
}

dependencies {
    coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:2.1.4'
}
```

Without this, your app will crash on devices running Android 7.x and below when the SDK attempts to use `java.time` classes like `Instant` and `ZonedDateTime`.

## Initialize the SDK

The first step to use the Kindly SDK is to initialize it with your bot key:

```kotlin
KindlySDK.start(
    application = application, 
    botKey = "YOUR_BOT_KEY", 
    languageCode = "en"
)
```

You can also specify the behavior type to choose between native UI and web-based UI:

```kotlin
// Use native Android UI (default)
KindlySDK.start(
    application = application,
    botKey = "YOUR_BOT_KEY",
    languageCode = "en",
    behaviorType = BehaviorType.NATIVE
)

// Use web-based UI in a WebView
KindlySDK.start(
    application = application,
    botKey = "YOUR_BOT_KEY",
    languageCode = "en",
    behaviorType = BehaviorType.WEB
)
```

You can also provide an authentication callback if your bot requires authentication:

```kotlin
KindlySDK.start(
    application = application,
    botKey = "YOUR_BOT_KEY",
    languageCode = "en",
    behaviorType = BehaviorType.NATIVE,
    authTokenCallback = { chatId, promise ->
        // Generate JWT token
        // On success, call
        promise.fulfill("JWT_TOKEN")
        // On error
        promise.reject(error)
    }
)
```

For more details on authentication, see [Using Authentication](authentication.md).

## Change Language at Runtime

You can change the language after initialization using:

```kotlin
KindlySDK.setLanguage("lt")  // Change to Lithuanian
```

This method will:
- Call the language switch API
- Update the cached settings
- Refresh UI translations
- Notify all UI components of the language change

The language setting will persist across chat sessions until explicitly changed.

## Behavior Types

The SDK supports two display modes:

### Native Behavior (Default)
- Uses native Android UI with Jetpack Compose
- Fully integrated with Material Design patterns
- Optimal performance and user experience
- Full feature support including themes and customization

### Web Behavior
- Displays the chat interface in a WebView
- Uses the same web-based chat interface as your website
- Consistent cross-platform appearance
- Automatic chat opening and closing
- JavaScript bridge for seamless integration

```kotlin
// Native behavior (recommended for most apps)
KindlySDK.start(
    application = application,
    botKey = "YOUR_BOT_KEY",
    behaviorType = BehaviorType.NATIVE
)

// Web behavior (for consistent web experience)
KindlySDK.start(
    application = application,
    botKey = "YOUR_BOT_KEY",
    behaviorType = BehaviorType.WEB
)
```

**Note:** The behavior type can only be set during SDK initialization and cannot be changed at runtime.

## Bot Switching and Reinitialization

The SDK intelligently handles bot switching and reinitialization scenarios when you call `start()` multiple times with different parameters.

## Automatic Bot Key Switching

When you call `start()` with a **different bot key**, the SDK automatically:

1. **Detects the change**: Compares the new bot key with the current one
2. **Performs cleanup**: Calls `endChat()` internally to end the current session  
3. **Resets state**: Clears SDK initialization flags and session data
4. **Reinitializes**: Connects to the new bot seamlessly

```kotlin
// First initialization
KindlySDK.start(application = this, botKey = "bot-key-1")
KindlySDK.launchChat(context = this)

// Later, switch to different bot - automatic cleanup happens
KindlySDK.start(application = this, botKey = "bot-key-2")  // SDK handles cleanup internally
KindlySDK.launchChat(context = this)
```

## Behavior Type Changes

When you call `start()` with the **same bot key but different behavior type**:

```kotlin
// Start with native behavior
KindlySDK.start(
    application = this,
    botKey = "YOUR_BOT_KEY", 
    behaviorType = BehaviorType.NATIVE
)

// Switch to web behavior with same bot - automatic reinitialization
KindlySDK.start(
    application = this,
    botKey = "YOUR_BOT_KEY", 
    behaviorType = BehaviorType.WEB
)
```

The SDK will automatically disconnect and reinitialize with the new behavior type.

## Connection State Handling

When the SDK is **disconnected** (e.g., after calling `endChat()`), calling `start()` with the same bot key and behavior type will **reconnect**:

```kotlin
KindlySDK.start(application = this, botKey = "YOUR_BOT_KEY")
KindlySDK.launchChat(context = this)

// Later, end the chat
KindlySDK.endChat()

// Reconnect with same bot key - allowed reconnection  
KindlySDK.start(application = this, botKey = "YOUR_BOT_KEY")  // Reconnects instead of skipping
KindlySDK.launchChat(context = this)
```

## Skipping Duplicate Initialization

The SDK **skips initialization** only when all conditions are met:
- Same bot key
- Same behavior type
- **Still connected** (not disconnected)

```kotlin
KindlySDK.start(application = this, botKey = "YOUR_BOT_KEY", behaviorType = BehaviorType.NATIVE)
// This second call will be skipped - no changes needed
KindlySDK.start(application = this, botKey = "YOUR_BOT_KEY", behaviorType = BehaviorType.NATIVE)
```

## Best Practices for Bot Switching

### Option 1: Automatic Switching (Recommended)
```kotlin
// SDK handles cleanup automatically
KindlySDK.start(application = this, botKey = "new-bot-key")
KindlySDK.launchChat(context = this)
```

### Option 2: Explicit End-then-Start
```kotlin
// Explicit control over the lifecycle
KindlySDK.endChat()
// endChat() completes synchronously, so you can immediately call start()
KindlySDK.start(application = this, botKey = "new-bot-key")
KindlySDK.launchChat(context = this)
```

Both approaches work identically - choose based on your preference for explicit vs automatic lifecycle management.