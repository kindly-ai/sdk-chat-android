# Link Interception

The Kindly SDK allows your app to intercept links that are clicked from within the chat UI. This lets you handle specific links yourself instead of letting the SDK open them.

## How It Works

When a link is clicked inside the SDK (bot message links, button links, source references, etc.), the SDK calls the `shouldHandleLink` method on your `KindlySDKInteraction` implementation **synchronously** before opening the link.

| Scenario | Result |
|----------|--------|
| Interface not set | SDK handles the link normally |
| `shouldHandleLink` returns `true` | SDK handles the link normally |
| `shouldHandleLink` returns `false` | SDK does **not** open the link — your app handles it |

## Implementation

**1. Set the interface:**

```kotlin
KindlySDK.`interface` = object : KindlySDKInteraction {
    override fun didPressButton(chatButton: ButtonChat, chatLog: List<MessageChat>) {
        // Handle button press if needed
    }

    override fun shouldHandleLink(url: String): Boolean {
        // Example: intercept links to your own domain
        if (url.contains("www.example.com")) {
            // Handle the link yourself (e.g., navigate within your app)
            navigateToInternalPage(url)
            return false // Tell the SDK not to open it
        }
        return true // Let the SDK handle all other links
    }
}
```

## Notes

- The `shouldHandleLink` method has a default implementation that returns `true`, so you only need to override it if you want to intercept links.
- The method is called synchronously — return your decision immediately.
- This applies to all links originating from within the SDK: bot message links, button links, inline HTML links, image links, carousel card links, source reference links, and maintenance page links.
