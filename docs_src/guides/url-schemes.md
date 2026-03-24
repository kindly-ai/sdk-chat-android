# URL Schemes

The Kindly SDK supports deep linking via custom URL schemes. This allows your app to open the chat and trigger specific dialogues from external links.

## Supported URL Format

```
kindly://chat/dialogue/{dialogue_id}
```

When handled, the SDK will:
- If already connected: trigger the dialogue immediately
- If started but not connected: open the chat UI, connect, and trigger the dialogue
- If not started: return `false` (you must call `start()` first)

## Register the URL Scheme

Add an intent filter to your Activity in `AndroidManifest.xml`:

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="kindly" android:host="chat" />
    </intent-filter>
</activity>
```

## Handle the URL

**In your Activity:**

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    handleIntent(intent)
}

override fun onNewIntent(intent: Intent) {
    super.onNewIntent(intent)
    handleIntent(intent)
}

private fun handleIntent(intent: Intent) {
    intent.data?.let { uri ->
        KindlySDK.handleUrl(context = this, uri = uri)
    }
}
```

## Return Value

`handleUrl()` returns `true` if the URL was recognized and handled by the SDK, `false` otherwise.
