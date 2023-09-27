# Kindly SDK for Android

This is the official Kindly SDK for Android. This SDK allows you to integrate Kindly's chat functionality into your Android application.

## Installation

Follow these steps to install the Kindly SDK into your Android project.

### Step 1: Add the JitPack repository to your build file

Add the following lines to your root `build.gradle` file:

```gradle
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

### Step 3: Initialize the SDK

In your Application class, initialize the SDK as follows:

```kotlin
class App: Application() {
    override fun onCreate() {
        super.onCreate()

        sdk = ChatKindlySDK(
            application = this,
            chatBotKey = "BOT_KEY",
            getAuthToken = null
        )
    }

    companion object {
        lateinit var sdk: ChatKindlySDK
    }
}
```

## Usage

After installing the SDK, you can now use it in your Android application. 

To launch the chat, call `sdk.launchChat(context = context)` in your activity or fragment:

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyApplicationTheme {
                Box(modifier = Modifier.fillMaxSize()) {
                    MyButton()
                }
            }
        }
    }
}

@Composable
fun MyButton() {
    val context = LocalContext.current
    Button(onClick = {
        sdk.launchChat(context = context)
                     },
        ) {
        Text(text = "Click Here")
    }
}
```

## Support

If you encounter any issues or require further assistance, feel free to create an issue in this repository or contact Kindly's support team.

## Additional Information

For more information, please refer to the following resources:

- [Documentation](https://github.com/kindly-ai/sdk-chat-android/wiki)
- [JitPack](https://jitpack.io/#kindly-ai/sdk-chat-android)