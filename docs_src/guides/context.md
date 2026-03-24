# Context

## Set context

```Kotlin
KindlySDK.setNewContext(context: Map<String, String>)
```

This context will be sent on **connect**, or next **message sent**, or **next button click**.

The context will be cleared automatically afterward

## Clear the context manually

```Kotlin
KindlySDK.clearNewContext()
```

