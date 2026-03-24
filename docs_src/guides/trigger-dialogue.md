# Trigger Dialogue

This displays the chat bot and triggers the dialogId provided

```kotlin
KindlySDK.launchChat(context = context, triggerDialogueId = "trigger_dialogue_id")
```

You can also trigger a dialogue manually using (this will not display the Kindly UI):

```kotlin
KindlySDK.triggerDialogue(id = "trigger_dialogue_id")
```
