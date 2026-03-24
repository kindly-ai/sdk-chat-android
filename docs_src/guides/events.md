# Events

The Kindly SDK provides a unified event system that allows you to subscribe to all SDK events and state changes in real-time. This matches the web API pattern and provides a comprehensive way to monitor SDK behavior using Kotlin Flows.

## Event Types

The SDK emits the following event types:

- **`kindly:load`** - Settings and configuration loaded
- **`kindly:message`** - New messages (sent/received)
- **`kindly:buttonclick`** - Button interactions
- **`kindly:ui`** - UI actions (open, close, end chat, etc.)
- **`kindly:state`** - State changes (connection, display status)

## Basic Usage

### Subscribe to All Events

```kotlin
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import no.kindly.chatsdk.chat.KindlySDK
import no.kindly.chatsdk.chat.models.KindlyEventDetail

class MyEventHandler {
    private val scope = CoroutineScope(Dispatchers.Main)
    
    fun setupEventListening() {
        scope.launch {
            KindlySDK.events.collect { event ->
                println("Event: ${event.type.value}")
                println("Timestamp: ${event.timestamp}")
                
                when (val detail = event.detail) {
                    is KindlyEventDetail.Load -> {
                        println("Settings loaded: ${detail.settings.name}")
                    }
                    
                    is KindlyEventDetail.Message -> {
                        println("New message: ${detail.newMessage.text ?: "No text"}")
                    }
                    
                    is KindlyEventDetail.ButtonClick -> {
                        println("Button clicked: ${detail.button.label}")
                    }
                    
                    is KindlyEventDetail.UI -> {
                        println("UI Action: ${detail.action.type.value}")
                    }
                    
                    is KindlyEventDetail.State -> {
                        println("State changed - Connected: ${detail.state.isConnected}")
                    }
                    
                    is KindlyEventDetail.GlobalIcon -> {
                        // Note: globalIcon events are not currently implemented in mobile SDKs
                    }
                }
            }
        }
    }
}
```

### Subscribe to State Changes Only

```kotlin
scope.launch {
    KindlySDK.state.collect { state ->
        println("Chat displayed: ${state.isChatDisplayed}")
        println("SDK started: ${state.isStarted}")
        println("Socket connected: ${state.isSocketConnected}")
        println("Fully connected: ${state.isConnected}")
        println("Connection state: ${state.connectionState}")
    }
}
```

## Event Details

### Load Event
Emitted when SDK settings are loaded from the server.

```kotlin
import kotlinx.coroutines.flow.filter
import no.kindly.chatsdk.chat.models.KindlyEventType

scope.launch {
    KindlySDK.events
        .filter { it.type == KindlyEventType.LOAD }
        .collect { event ->
            val detail = event.detail as? KindlyEventDetail.Load
            detail?.let {
                println("Bot: ${it.settings.name} (ID: ${it.settings.id})")
                println("Languages: ${it.settings.languages.map { lang -> lang.code }}")
                val hasCustomBackground = it.settings.style.background != null
                println("Theme: ${if (hasCustomBackground) "Custom" else "Default"}")
            }
        }
}
```

### Message Event
Emitted for both incoming and outgoing messages.

```kotlin
scope.launch {
    KindlySDK.events
        .filter { it.type == KindlyEventType.MESSAGE }
        .collect { event ->
            val detail = event.detail as? KindlyEventDetail.Message
            detail?.let {
                println("Message from ${it.newMessage.from}: ${it.newMessage.text ?: ""}")
                println("Chat has ${it.chatLog.size} total messages")
            }
        }
}
```

### Button Click Event
Emitted when users interact with chat buttons.

```kotlin
scope.launch {
    KindlySDK.events
        .filter { it.type == KindlyEventType.BUTTON_CLICK }
        .collect { event ->
            val detail = event.detail as? KindlyEventDetail.ButtonClick
            detail?.let {
                println("Button: ${it.button.label}")
                println("Type: ${it.buttonType}")
                println("Value: ${it.button.value ?: ""}")
            }
        }
}
```

### UI Events
Emitted for user interface actions like opening/closing chat, language changes, etc.

```kotlin
import no.kindly.chatsdk.chat.models.KindlyUIActionType

scope.launch {
    KindlySDK.events
        .filter { it.type == KindlyEventType.UI }
        .collect { event ->
            val detail = event.detail as? KindlyEventDetail.UI
            detail?.let { uiDetail ->
                when (uiDetail.action.type) {
                    KindlyUIActionType.OPEN_CHAT_BUBBLE -> {
                        println("Chat opened")
                    }
                    KindlyUIActionType.CLOSE_CHAT_BUBBLE -> {
                        println("Chat closed")
                    }
                    KindlyUIActionType.END_CHAT -> {
                        println("Chat ended")
                    }
                    KindlyUIActionType.RESTART_CHAT -> {
                        println("Chat restarted")
                    }
                    KindlyUIActionType.CHANGE_LANGUAGE -> {
                        println("Language changed to: ${uiDetail.action.languageCode ?: ""}")
                    }
                    KindlyUIActionType.DOWNLOAD_CHAT -> {
                        println("Chat downloaded")
                    }
                    KindlyUIActionType.DELETE_CHAT -> {
                        println("Chat deleted")
                    }
                    KindlyUIActionType.SHOW_FEEDBACK -> {
                        println("Feedback form shown")
                    }
                    KindlyUIActionType.SUBMIT_FEEDBACK -> {
                        println("Feedback submitted")
                    }
                    KindlyUIActionType.DISMISS_FEEDBACK -> {
                        println("Feedback dismissed")
                    }
                }
            }
        }
}
```

### State Events
Emitted when SDK connection or display state changes.

```kotlin
scope.launch {
    KindlySDK.state.collect { state ->
        when {
            state.isConnected -> {
                println("✅ SDK is fully connected and ready!")
            }
            state.isStarted && !state.isSocketConnected -> {
                println("⚠️ SDK started but socket disconnected")
            }
            !state.isStarted -> {
                println("⏳ SDK not started yet")
            }
        }
    }
}
```

## Advanced Usage

### Track Chat Display Changes

```kotlin
import kotlinx.coroutines.flow.distinctUntilChanged
import kotlinx.coroutines.flow.map

scope.launch {
    KindlySDK.state
        .map { it.isChatDisplayed }
        .distinctUntilChanged() // Only emit when value changes
        .collect { isDisplayed ->
            if (isDisplayed) {
                println("📱 Chat interface is now visible")
            } else {
                println("🙈 Chat interface is now hidden")
            }
        }
}
```

### Monitor Connection Health

```kotlin
scope.launch {
    KindlySDK.state
        .map { state -> Triple(state.isStarted, state.isSocketConnected, state.isConnected) }
        .distinctUntilChanged()
        .collect { (isStarted, isSocketConnected, isConnected) ->
            when {
                isStarted && isSocketConnected && isConnected -> {
                    println("🟢 Fully connected")
                }
                isStarted && !isSocketConnected -> {
                    println("🟡 Started but disconnected")
                }
                !isStarted -> {
                    println("🔴 Not started")
                }
                else -> {
                    println("🟡 Partial connection")
                }
            }
        }
}
```

### Filter Multiple Event Types

```kotlin
scope.launch {
    KindlySDK.events
        .filter { event ->
            listOf(
                KindlyEventType.MESSAGE,
                KindlyEventType.BUTTON_CLICK,
                KindlyEventType.UI
            ).contains(event.type)
        }
        .collect { event ->
            println("User interaction event: ${event.type.value}")
        }
}
```

### Combine Events with Other Flows

```kotlin
import kotlinx.coroutines.flow.combine

scope.launch {
    combine(
        KindlySDK.state,
        KindlySDK.events.filter { it.type == KindlyEventType.MESSAGE }
    ) { state, messageEvent ->
        Pair(state, messageEvent)
    }.collect { (state, messageEvent) ->
        if (state.isConnected) {
            println("Received message while connected: $messageEvent")
        }
    }
}
```

## Lifecycle Management

### In Activities/Fragments

```kotlin
class ChatActivity : AppCompatActivity() {
    private val scope = CoroutineScope(Dispatchers.Main + SupervisorJob())
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // Setup event listening
        scope.launch {
            KindlySDK.events.collect { event ->
                // Handle events
            }
        }
    }
    
    override fun onDestroy() {
        super.onDestroy()
        scope.cancel() // Cancel all coroutines
    }
}
```

### In ViewModels

```kotlin
class ChatViewModel : ViewModel() {
    
    init {
        // Subscribe to events in ViewModel scope
        viewModelScope.launch {
            KindlySDK.events.collect { event ->
                // Handle events
            }
        }
    }
}
```

## Event Data Models

### KindlyEvent
```kotlin
data class KindlyEvent(
    val type: KindlyEventType,
    val timestamp: Long,
    val detail: KindlyEventDetail
)
```

### KindlyEventState
```kotlin
data class KindlyEventState(
    val isChatDisplayed: Boolean,
    val isStarted: Boolean,
    val isSocketConnected: Boolean,
    val isConnected: Boolean,
    val connectionState: String
)
```

### Event Types Enum
```kotlin
enum class KindlyEventType(val value: String) {
    LOAD("kindly:load"),
    MESSAGE("kindly:message"),
    BUTTON_CLICK("kindly:buttonclick"),
    UI("kindly:ui"),
    STATE("kindly:state")
    // Note: GLOBAL_ICON is defined but not implemented in mobile SDKs
}
```

For more detailed information about event data structures, see the SDK source code documentation.

## Best Practices

1. **Use appropriate scope**: Use `viewModelScope` in ViewModels, activity/fragment lifecycle scope in UI components
2. **Handle cancellation**: Always cancel coroutines when they're no longer needed
3. **Use Flow operators**: Leverage `filter`, `map`, `distinctUntilChanged` to process events efficiently
4. **Error handling**: Wrap event collection in try-catch blocks for production apps
5. **Performance**: Use `conflate()` or `sample()` for high-frequency events if needed

## Error Handling

```kotlin
scope.launch {
    try {
        KindlySDK.events.collect { event ->
            // Handle events
        }
    } catch (e: Exception) {
        Log.e("EventHandler", "Error collecting events", e)
    }
}
``` 