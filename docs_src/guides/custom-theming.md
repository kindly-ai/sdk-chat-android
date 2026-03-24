# Custom Theming

Customize the appearance of the Kindly chat interface by setting your own color theme. The SDK provides two methods for theme customization:

1. Using a complete `KindlyThemeColors` object
2. Specifying individual colors

## Setting a Complete Theme

Create and configure a custom theme using the provided `CustomKindlyTheme` class:

```kotlin
// Create a custom theme
val darkTheme = CustomKindlyTheme(
    background = Color.Black,
    botMessageBackground = Color(0xFF333333),          // Dark gray
    botMessageText = Color.White,
    bubbleButtonBackground = Color(0xFF3700B3),        // Purple
    buttonBackground = Color(0xFFF44336),              // Red
    buttonText = Color.White, 
    userMessageBackground = Color(0xFF4CAF50),         // Green
    userMessageText = Color.White,
    headerBackground = Color(0xFF1A1A1A),              // Very dark gray
    headerText = Color.White,
    inputBackground = Color(0xFF333333),               // Dark gray
    inputText = Color.White,
    chatLogElements = Color(0xFFBBBBBB)                // Light gray
    // Add other color properties as needed
)

// Apply the custom theme
KindlySDK.setCustomTheme(darkTheme)
```

## Setting Individual Colors

For convenience, you can also specify just the colors you want to customize without creating a complete theme:

```kotlin
KindlySDK.setCustomTheme(
    background = Color.White,
    botMessageBackground = Color(0xFFE8E8E8),          // Light gray
    botMessageText = Color(0xFF333333),                // Dark gray
    bubbleButtonBackground = Color(0xFF3F51B5),        // Indigo
    buttonBackground = Color(0xFFF44336),              // Red
    buttonText = Color.White,
    userMessageBackground = Color(0xFF4CAF50),         // Green
    userMessageText = Color.White,
    headerBackground = Color(0xFF3F51B5),              // Indigo
    headerText = Color.White,
    chatLogElements = Color(0xFF808080)                // Gray
    // Only specify the colors you want to customize
)
```

## Available Theme Properties

The theme properties are divided into two categories:

### API-defined Colors (from Kindly settings)

These colors are defined in the Kindly settings (app.kindly.ai) and are fetched with your bot configuration:

| Property | Description |
|----------|-------------|
| `background` | Main background color of the chat interface |
| `botMessageBackground` | Background color for bot message bubbles |
| `botMessageText` | Text color for bot messages |
| `bubbleButtonBackground` | Background color for the chat bubble button |
| `buttonBackground` | Background color for buttons |
| `buttonOutline` | Outline color for buttons |
| `buttonText` | Text color for buttons |
| `chatBubbleText` | Text color for the chat bubble |
| `chatLogElements` | Color for chat log elements like timestamps |
| `globalIconsButtonBackground` | Background color for global icons buttons |
| `headerBackground` | Background color for the header |
| `headerText` | Text color for the header |
| `inputBackground` | Background color for the input field |
| `inputText` | Text color for the input field |
| `panelBackground` | Background color for panels |
| `secondaryButtonBackground` | Background color for secondary buttons |
| `userMessageBackground` | Background color for user message bubbles |
| `userMessageText` | Text color for user messages |

### Manually Added Colors

These colors are not defined in the Kindly settings but are used by the SDK for additional UI elements:

| Property | Description |
|----------|-------------|
| `buttonSelectedBackground` | Background color for selected buttons |
| `defaultShadow` | Default shadow color |
| `inputCursor` | Color for the input cursor |
| `maintenanceHeaderBackground` | Background color for maintenance alerts |

## Clearing Custom Theme

To clear a custom theme and revert to the default or API-provided theme:

```kotlin
KindlySDK.clearCustomTheme()
```

This will:
- ✅ Remove any user-defined custom theme
- ✅ Revert to the theme from Kindly settings (if available)
- ✅ Fall back to the default SDK theme if no API theme exists
- ✅ Apply changes immediately to any displayed chat interface

## Theme Priority

When setting a custom theme:

1. **User-defined theme** (set via `setCustomTheme`) takes precedence over any other theme
2. **API theme** (from Kindly settings configured in app.kindly.ai) is used if no user theme is set
3. **Default SDK theme** is applied if neither user nor API themes are available

### Theme Priority Examples

```kotlin
// Set a custom theme (highest priority)
KindlySDK.setCustomTheme(
    background = Color.Black,
    botMessageBackground = Color(0xFF333333)
)

// Later, clear custom theme to use API theme
KindlySDK.clearCustomTheme()  // Reverts to API or default theme

// Set custom theme again (overrides API theme)
KindlySDK.setCustomTheme(background = Color.White)
```

## Implementation in App.kt

Here's an example of applying a custom theme in your application class:

```kotlin
override fun onCreate() {
    super.onCreate()
    
    // Initialize the SDK
    KindlySDK.start(
        application = this,
        botKey = YOUR_BOT_KEY,
        languageCode = "en"
    )
    
    // Apply a custom theme
    KindlySDK.setCustomTheme(
        background = Color.White,
        botMessageBackground = Color(0xFFE8E8E8),
        botMessageText = Color(0xFF333333),
        buttonBackground = Color(0xFFF44336),
        buttonText = Color.White,
        userMessageBackground = Color(0xFF4CAF50),
        userMessageText = Color.White
    )
    
    // Rest of your initialization code
} 