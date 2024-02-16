# Kindly SDK for Android

This is the official Kindly SDK for Android. This SDK allows you to integrate Kindly's chat functionality into your Android application.

## Installation

Follow these steps to install the Kindly SDK into your Android project.

### Step 1: Add the JitPack repository to your build file

#### Groovy

Add the following lines to your root `build.gradle` file:

```groovy
allprojects {
    repositories {
        ...
        maven { url "https://jitpack.io" }
    }
}
```

#### Kotlin DSL

```kotlin
pluginManagement {
    repositories {
        ...
        maven(url = "https://jitpack.io")
    }
}
```

### Step 2: Add the dependency

#### Groovy

```groovy
dependencies {
	implementation "com.github.kindly-ai:sdk-chat-android:TAG"
}
```

#### Kotlin DSL

```kotlin
dependencies {
	implementation("com.github.kindly-ai:sdk-chat-android:TAG")
}
```

Find the latest version [here](https://jitpack.io/#kindly-ai/sdk-chat-android)

### Step 3: Initialize the SDK

In your Application class, initialize the SDK as follows:

```kotlin
class App: Application() {
    override fun onCreate() {
        super.onCreate()
				
      	// 🌿 Initialize the SDK
        KindlySDK.start(
            application = this,
            chatBotKey = "BOT_KEY",
            getAuthToken = null
        )
    }
}
```

## Usage

After installing the SDK, you can now use it in your Android application. 

To launch the chat, call `KindlySDK.launchChat(context = context)` in your activity or fragment:

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
      	// 🌿 Display the SDK screen
        KindlySDK.launchChat(context = context)
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

- [Getting Started](https://github.com/kindly-ai/sdk-chat-android/wiki)
- [📚 Documentation](https://kindly-ai.github.io/sdk-chat-android/)
- [JitPack](https://jitpack.io/#kindly-ai/sdk-chat-android)