# Kill SDK

## Completely terminate the SDK
```Kotlin
KindlySDK.kill()
```

The `kill()` method **completely terminates the SDK** and clears all state. This is more destructive than [`endChat()`](end-chat.md) which only disconnects the chat session.

## What kill() does

When you call `kill()`, the SDK will:
- 🔄 **Disconnect the chat session** (same as `endChat()`)
- 🧹 **Clear all session data** including messages and context
- ⚡ **Reset SDK initialization state** 
- 🔑 **Clear bot key and authentication** 
- 🎨 **Reset configuration and themes**
- 📡 **Emit termination events** for cleanup

## After calling kill()

After calling `kill()`, the SDK returns to an **uninitialized state**:
- ❌ You **cannot** call `launchChat()` or other SDK methods
- ✅ You **must** call `start()` again before using any SDK functions
- ✅ All configuration (themes, permissions, etc.) needs to be set up again

## Usage Examples

## Complete SDK Shutdown
```Kotlin
// Terminate the SDK completely
KindlySDK.kill()
println("✅ SDK completely terminated")
// SDK is now in uninitialized state
```

## Switch to Different Bot (Alternative Method)
```Kotlin
// Method 1: Using kill() for explicit control
KindlySDK.kill()
// SDK is now uninitialized - must call start() again
KindlySDK.start(
    application = this,
    botKey = "new-bot-key",
    languageCode = "en"
)
KindlySDK.launchChat(context = this)

// Method 2: Automatic switching (recommended)
KindlySDK.start(
    application = this,
    botKey = "new-bot-key",  // SDK handles cleanup automatically
    languageCode = "en"
)
KindlySDK.launchChat(context = this)
```

## When to Use kill() vs endChat()

## Use `kill()` when:
- 🔄 **App shutdown**: Terminating the app or major state changes
- 🔀 **Complete reset**: Need to clear all SDK state and start fresh
- 🐛 **Error recovery**: Recovering from SDK errors or corruption
- 🧪 **Testing**: Ensuring clean state between test runs

## Use `endChat()` when:
- 💬 **End conversation**: User finishes chatting but may return later
- 🔄 **Temporary disconnect**: Preserving SDK state for quick reconnection
- 🎯 **Normal flow**: Standard chat lifecycle management

## Comparison

| Method | Session | SDK State | Bot Key | Config | Next Action |
|--------|---------|-----------|---------|--------|-------------|
| `endChat()` | ❌ Cleared | ✅ Preserved | ✅ Preserved | ✅ Preserved | `launchChat()` |
| `kill()` | ❌ Cleared | ❌ Reset | ❌ Cleared | ❌ Reset | `start()` required |

## Synchronous Operation

The `kill()` method completes **synchronously**, ensuring all cleanup is finished before returning:

```Kotlin
KindlySDK.kill()
// Safe to reinitialize immediately after this line
KindlySDK.start(
    application = this,
    botKey = "new-bot-key",
    languageCode = "en"
)
```

## Best Practices

1. **Safe to call immediately** after `kill()`:
   ```Kotlin
   KindlySDK.kill()
   // Immediately safe to reinitialize
   KindlySDK.start(application = this, botKey = "new-bot")
   ```

2. **Use automatic bot switching** instead of manual kill/start when possible:
   ```Kotlin
   // Preferred: Automatic cleanup
   KindlySDK.start(application = this, botKey = "new-bot")
   
   // Instead of: Manual cleanup  
   KindlySDK.kill()
   KindlySDK.start(application = this, botKey = "new-bot")
   ```

3. **Handle errors gracefully** in production apps:
   ```Kotlin
   try {
       KindlySDK.kill()
       // Continue with app flow
   } catch (e: Exception) {
       Log.e("SDK", "Kill failed, but continuing", e)
   }
   ```

## Thread Safety

The `kill()` method is **thread-safe** and can be called from any thread. However, subsequent SDK operations should follow the main thread requirements of the Android SDK.

For normal chat lifecycle management, prefer [`endChat()`](end-chat.md) over `kill()`.
