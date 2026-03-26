# Kindly SDK for Android

This is the official Kindly SDK for Android. This SDK allows you to integrate Kindly's chat functionality into your Android application.

## Installation

### Option A: Maven Central (Recommended)

No extra repository configuration needed — Maven Central is included by default in Android projects.

#### Groovy

```groovy
dependencies {
    implementation 'ai.kindly:sdk:TAG'
}
```

#### Kotlin DSL

```kotlin
dependencies {
    implementation("ai.kindly:sdk:TAG")
}
```

Find the latest version on [Maven Central](https://central.sonatype.com/artifact/ai.kindly/sdk).

### Option B: JitPack

Add the JitPack repository to your build file:

#### Groovy

```groovy
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
    }
}
```

#### Kotlin DSL

```kotlin
dependencyResolutionManagement {
    repositories {
        maven(url = "https://jitpack.io")
    }
}
```

Then add the dependency:

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

Find the latest version on [JitPack](https://jitpack.io/#kindly-ai/sdk-chat-android).

### Step 3: Initialize the SDK

In your Application class, initialize the SDK as follows:

```kotlin
class App: Application() {
    override fun onCreate() {
        super.onCreate()
				
      	// 🌿 Initialize the SDK
        KindlySDK.start(
            application = this,
            botKey = "BOT_KEY",
            languageCode = "en",
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
    },) {
        Text(text = "Click Here")
    }
}
```

## Support

If you encounter any issues or require further assistance, feel free to create an issue in this repository or contact Kindly's support team.

## Documentation

- [Getting Started & Guides](https://kindly-ai.github.io/sdk-chat-android-sources/)
- [API Reference](https://kindly-ai.github.io/sdk-chat-android-sources/api-reference/)
- [JitPack](https://jitpack.io/#kindly-ai/sdk-chat-android)