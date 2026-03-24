# Authentication

## Setup

To use authentication with the Kindly SDK, you can initialize the SDK by adding an argument to the `start` method. Here's an example of how to set the `authTokenCallback` using the `start` method:

`KindlySDK.start(botKey: String, authTokenCallback: ((chatId: String) -> Deferred<String>)? = null)`

```Kotlin
KindlySDK.start(
    application = this,
    botKey = "BOT_KEY",
    languageCode = "en",
    authTokenCallback = { chatId ->
        val deferredToken = CompletableDeferred<String>()

        // Async code
        /*** om completion call 
        deferredToken.complete(token)
        ***/
        

        deferredToken
    }
)
```

Inside the `authTokenCallback` closure, you can generate a JWT token and fulfill the Deferred with the token on success.
