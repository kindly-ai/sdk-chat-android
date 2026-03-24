# Notification Support

## Kindly notification support in handover

First provide the SDK with the FCM token using:

```Kotlin
KindlySDK.saveNotificationToken(token = "firebase_token")
```

## When a push notification is received

Forward the notification to SDK using

```Kotlin
KindlySDK.handleNotification(remoteMessage = remoteMessage)
```

The SDK will double-check if notification type is a Kindly notification before handling it

## Check if a notification is from Kindly

To determine if a notification should be handled by the Kindly SDK or another notification provider, use the following helper method:

```Kotlin
val isKindlyNotification = KindlySDK.isKindlyNotification(remoteMessage)
```

This can be particularly useful when integrating with multiple push notification providers. Example usage:

```Kotlin
override fun onMessageReceived(remoteMessage: RemoteMessage) {
    if (KindlySDK.isKindlyNotification(remoteMessage)) {
        // Handle with Kindly SDK
        KindlySDK.handleNotification(remoteMessage)
    } else {
        // Handle with another notification provider (e.g., Braze, mParticle)
        otherNotificationProvider.handleNotification(remoteMessage)
    }
}
```

