# Kindly Android SDK

The Kindly Chat SDK for Android allows you to integrate a fully-featured customer support chat into your Android application. The SDK provides a native Jetpack Compose chat interface with real-time messaging, push notifications, custom theming, and more.

## Installation

### Step 1: Add the JitPack repository

Add JitPack to your root `build.gradle` or `settings.gradle`:

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    repositories {
        maven(url = "https://jitpack.io")
    }
}
```

### Step 2: Add the dependency

```kotlin
dependencies {
    implementation("com.github.kindly-ai:sdk-chat-android:TAG")
}
```

Find the latest version on [JitPack](https://jitpack.io/#kindly-ai/sdk-chat-android).

## Quick Start

```kotlin
// Initialize in your Application class
KindlySDK.start(
    application = this,
    botKey = "YOUR_BOT_KEY",
    languageCode = "en"
)

// Display the chat from any Activity/Fragment
KindlySDK.launchChat(context = context)
```

## Documentation

<div class="grid cards" markdown>

-   **Getting Started**

    ---

    Set up the SDK and configure it for your app

    [:octicons-arrow-right-24: Initialization](guides/initialization.md)
    [:octicons-arrow-right-24: Configuration](guides/config.md)

-   **Chat Lifecycle**

    ---

    Control how the chat UI is displayed and managed

    [:octicons-arrow-right-24: Launch Chat](guides/launch-chat.md)
    [:octicons-arrow-right-24: Close Chat](guides/close-chat.md)
    [:octicons-arrow-right-24: End Chat](guides/end-chat.md)
    [:octicons-arrow-right-24: Kill SDK](guides/kill-sdk.md)

-   **Features**

    ---

    Events, authentication, context, dialogues, and notifications

    [:octicons-arrow-right-24: Events](guides/events.md)
    [:octicons-arrow-right-24: Authentication](guides/authentication.md)
    [:octicons-arrow-right-24: Notifications](guides/notification-support.md)

-   **Customization**

    ---

    Theme the chat UI, handle links, and set up deep linking

    [:octicons-arrow-right-24: Custom Theming](guides/custom-theming.md)
    [:octicons-arrow-right-24: URL Schemes](guides/url-schemes.md)
    [:octicons-arrow-right-24: Link Interception](guides/link-interception.md)

</div>

## API Reference

Auto-generated documentation for all public classes, interfaces, enums, and data classes.

[:octicons-book-24: Browse API Reference](api-reference/index.md){ .md-button }
