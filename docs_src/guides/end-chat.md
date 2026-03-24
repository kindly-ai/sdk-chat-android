# End Chat

## Disconnect the chat

Ends the chat and disconnects the SDK.
_clears the context as well_

```Kotlin
KindlySDK.endChat()
```

**Important:** The `endChat()` method now **preserves the SDK initialization state**. This means:
- ✅ The SDK remains initialized and ready for use
- ✅ You can call `launchChat()` again without needing to call `start()` 
- ✅ Bot key, authentication, and configuration are preserved
- ✅ Only the active chat session and context are cleared

If you need to **completely terminate the SDK**, use [`kill()`](kill-sdk.md) instead.

## Bot Switching Workflows

When switching between different bots, you have two workflow options:

### Option 1: Automatic Switching (Recommended)

The SDK automatically handles cleanup when you switch bot keys:

```kotlin
// Current bot
KindlySDK.start(application = this, botKey = "bot-1")
KindlySDK.launchChat(context = this)

// Switch to different bot - no explicit endChat() needed
KindlySDK.start(application = this, botKey = "bot-2")  // SDK handles cleanup internally
KindlySDK.launchChat(context = this)
```

### Option 2: Explicit End-then-Start

For explicit control over the chat lifecycle:

```kotlin
// End current chat first
KindlySDK.endChat()
// endChat() completes synchronously, so you can immediately proceed
KindlySDK.start(application = this, botKey = "new-bot-key")
KindlySDK.launchChat(context = this)
```

**Both approaches work identically** - the SDK ensures proper cleanup and reinitialization in both cases.

## When to Use endChat()

Use `endChat()` explicitly when you want to:
- **Disconnect completely**: Stop the chat service without switching to another bot
- **Clear session data**: Ensure all messages and context are cleared  
- **Explicit lifecycle control**: Have fine-grained control over connection timing

The `endChat()` method completes synchronously, ensuring all cleanup is finished before returning.

