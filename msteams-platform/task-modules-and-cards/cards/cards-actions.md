---
title: Add card actions in a bot
description: Describes card actions in Microsoft Teams and how to use them in your bots
ms.localizationpriority: medium
ms.topic: conceptual
keywords: teams bots cards actions
---

# Card actions

Cards used by bots and messaging extensions in Teams support the following activity [`CardAction`](/bot-framework/dotnet/bot-builder-dotnet-add-rich-card-attachments#process-events-within-rich-cards) types:

> [!NOTE]
> The `CardAction` actions differ from `potentialActions` for Office 365 Connector cards when used from connectors.

| Type | Action |
| --- | --- |
| `openUrl` | Opens a URL in the default browser. |
| `messageBack` | Sends a message and payload to the bot from the user who selected the button or tapped the card. Sends a separate message to the chat stream. |
| `imBack`| Sends a message to the bot from the user who selected the button or tapped the card. This message from user to bot is visible to all conversation participants. |
| `invoke` | Sends a message and payload to the bot from the user who selected the button or tapped the card. This message is not visible. |
| `signin` | Initiates OAuth flow, allowing bots to connect with secure services. |

> [!NOTE]
>* Teams does not support `CardAction` types not listed in the previous table.
>* Teams does not support the `potentialActions` property.
>* Card actions are different than [suggested actions](/azure/bot-service/bot-builder-howto-add-suggested-actions?view=azure-bot-service-4.0&tabs=javascript#suggest-action-using-button&preserve-view=true) in Bot Framework or Azure Bot Service. Suggested actions are not supported in Microsoft Teams. If you want buttons to appear on a Teams bot message, use a card.
>* If you are using a card action as part of a messaging extension, the actions do not work until the card is submitted to the channel. The actions do not work while the card is in the compose message box.

## Action type openUrl

`openUrl` action type specifies a URL to launch in the default browser.

> [!NOTE]
> Your bot does not receive any notice on which button was selected.

With `openUrl`, you can create an action with the following properties:

| Property | Description |
| --- | --- |
| `title` | Appears as the button label. |
| `value` | This field must contain a full and properly formed URL. |

# [JSON](#tab/json)

The following code shows an example of `openUrl` action type in JSON:

```json
{
    "type": "openUrl",
    "title": "Tabs in Teams",
    "value": "https://msdn.microsoft.com/microsoft-teams/tabs"
}
```

# [C#](#tab/csharp)

The following code shows an example of `openUrl` action type in C#:

```csharp
var button = new CardAction()
{
    Type = ActionTypes.OpenUrl,
    Title = "Tabs in Teams",
    Value = "https://docs.microsoft.com/en-us/microsoftteams/platform/"
};
```

# [JavaScript/Node.js](#tab/javascript)

The following code shows an example of `openUrl` action type in JavaScript:

```javascript
CardFactory.actions([
{
    type: 'openUrl',
    title: 'Tabs in Teams',
    value: 'https://docs.microsoft.com/en-us/microsoftteams/platform/'
}])
```

---

## Action type messageBack

With `messageBack`, you can create a fully customized action with the following properties:

| Property | Description |
| --- | --- |
| `title` | Appears as the button label. |
| `displayText` | Optional. Used by the user in the chat stream when the action is performed. This text is not sent to your bot. |
| `value` | Sent to your bot when the action is performed. You can encode context for the action, such as unique identifiers or a JSON object. |
| `text` | Sent to your bot when the action is performed. Use this property to simplify bot development. Your code can check a single top-level property to dispatch bot logic. |

The flexibility of `messageBack` means that your code cannot leave a visible user message in the history simply by not using `displayText`.

# [JSON](#tab/json)

The following code shows an example of `messageBack` action type in JSON:

```json
{
  "buttons": [
    {
    "type": "messageBack",
    "title": "My MessageBack button",
    "displayText": "I clicked this button",
    "text": "User just clicked the MessageBack button",
    "value": "{\"property\": \"propertyValue\" }"
    }
  ]
}
```

The `value` property can be either a serialized JSON string or a JSON object.

# [C#](#tab/csharp)

The following code shows an example of `messageBack` action type in C#:

```csharp
var button = new CardAction()
{
    Type = ActionTypes.MessageBack,
    Title = "My MessageBack button",
    DisplayText = "I clicked this button",
    Text = "User just clicked the MessageBack button",
    Value = "{\"property\": \"propertyValue\" }"
};
```

# [JavaScript/Node.js](#tab/javascript)

The following code shows an example of `messageBack` action type in JavaScript:

```javascript
CardFactory.actions([
{
    type: 'messageBack',
    title: "My MessageBack button",
    displayText: "I clicked this button",
    text: "User just clicked the MessageBack button",
    value: {property: "propertyValue" }
}])
```

---

### Inbound message example

`replyToId` contains the ID of the message that the card action came from. Use it if you want to update the message.

The following code shows an example of inbound message:

```json
{
   "text":"User just clicked the MessageBack button",
   "value":{
      "property":"propertyValue"
   },
   "type":"message",
   "timestamp":"2017-06-22T22:38:47.407Z",
   "id":"f:5261769396935243054",
   "channelId":"msteams",
   "serviceUrl":"https://smba.trafficmanager.net/amer-client-ss.msg/",
   "from":{
      "id":"29:102jd210jd010icsoaeclaejcoa9ue09u",
      "name":"John Smith"
   },
   "conversation":{
      "id":"19:malejcou081i20ojmlcau0@thread.skype;messageid=1498171086622"
   },
   "recipient":{
      "id":"28:76096e45-119f-4736-859c-6dfff54395f7",
      "name":"MyBot"
   },
   "entities":[
      {
        "locale": "en-US",
        "country": "US",
        "platform": "Windows",
        "timezone": "America/Los_Angeles",
        "type": "clientInfo" 
      }
   ],
   "channelData":{
      "channel":{
         "id":"19:malejcou081i20ojmlcau0@thread.skype"
      },
      "team":{
         "id":"19:12d021jdoijsaeoaue0u@thread.skype"
      },
      "tenant":{
         "id":"bec8e231-67ad-484e-87f4-3e5438390a77"
      }
   },
        "replyToId": "1575667808184",
}
```

## Action type imBack

The `imBack` action triggers a return message to your bot, as if the user typed it in a normal chat message. Your user and all other users in a channel can see the button response.

With `imBack`, you can create an action with the following properties:

| Property | Description |
| --- | --- |
| `title` | Appears as the button label. |
| `value` | This field must contain the text string used in the chat and therefore sent back to the bot. This is the message text you process in your bot to perform the desired logic. |

> [!NOTE]
> The `value` field is a simple string. There is no support for formatting or hidden characters.

# [JSON](#tab/json)

The following code shows an example of `imBack` action type in JSON:

```json
{
    "type": "imBack",
    "title": "More",
    "value": "Show me more"
}
```

# [C#](#tab/csharp)

The following code shows an example of `imBack` action type in C#:

```csharp
var button = new CardAction()
{
    Type = ActionTypes.ImBack,
    Title = "More",
    Value = "Show me more"
};
```

# [JavaScript/Node.js](#tab/javascript)

The following code shows an example of `imBack` action type in JavaScript:

```javascript
CardFactory.actions([
{
    type: "imBack",
    title: "More",
    value: "Show me more"
}])
```

---

## Action type invoke

The `invoke` action is used for invoking [task modules](~/task-modules-and-cards/task-modules/task-modules-bots.md).

The `invoke` action contains three properties, `type`, `title`, and `value`.

With `invoke`, you can create an action with the following properties:

| Property | Description |
| --- | --- |
| `title` | Appears as the button label. |
| `value` | This property can contain a string, a stringified JSON object, or a JSON object. |

# [JSON](#tab/json)

The following code shows an example of `invoke` action type in JSON:

```json
{
    "type": "invoke",
    "title": "Option 1",
    "value": {
        "option": "opt1"
    }
}
```

When a user selects the button, your bot receives the `value` object with some additional information.

> [!NOTE]
> The activity type is `invoke` instead of `message` that is `activity.Type == "invoke"`.

# [C#](#tab/csharp)

The following code shows an example of `invoke` action type in C#:

```csharp
var button = new CardAction()
{
    Title = "Option 1",
    Type = "invoke",
    Value = "{\"option\": \"opt1\"}"
};
```

# [JavaScript/Node.js](#tab/javascript)

The following code shows an example of `invoke` action type in Node.js:

```javascript
CardFactory.actions([
{
    type: "invoke",
    title: "Option 1",
    value: {
        option: "opt1"
    }
}])
```

---

### Example of incoming invoke message

The top-level `replyToId` property contains the ID of the message that the card action came from. Use it if you want to update the message.

The following code shows an example of incoming invoke message:

```json
{
    "type": "invoke",
    "value": {
        "option": "opt1"
    },
    "timestamp": "2017-02-10T04:11:19.614Z",
    "localTimestamp": "2017-02-09T21:11:19.614-07:00",
    "id": "f:6894910862892785420",
    "channelId": "msteams",
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
    "from": {
        "id": "29:1Eniglq0-uVL83xNB9GU6w_G5a4SZF0gcJLprZzhtEbel21G_5h-
    NgoprRw45mP0AXUIZVeqrsIHSYV4ntgfVJQ",
        "name": "John Doe"
    },
    "conversation": {
        "id": "19:97b1ec61-45bf-453c-9059-6e8984e0cef4_8d88f59b-ae61-4300-bec0-caace7d28446@unq.gbl.spaces"
    },
    "recipient": {
        "id": "28:8d88f59b-ae61-4300-bec0-caace7d28446",
        "name": "MyBot"
    },
    "entities": [
        {
            "locale": "en-US",
            "country": "US",
            "platform": "Web",
            "type": "clientInfo"
        }
    ],
    "channelData": {
        "channel": {
            "id": "19:dc5ba12695be4eb7bf457cad6b4709eb@thread.skype"
        },
        "team": {
            "id": "19:712c61d0ef384e5fa681ba90ca943398@thread.skype"
        },
        "tenant": {
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
        }
    },
    "replyToId": "1575667808184"
}
```

## Action type signin

`signin` action type initiates an OAuth flow that permits bots to connect with secure services. For more information, see [authentication flow in bots](~/bots/how-to/authentication/auth-flow-bot.md).

Teams also supports [Adaptive Cards actions](#adaptive-cards-actions) that are only used by Adaptive Cards.

# [JSON](#tab/json)

The following code shows an example of `signin` action type in JSON:

```json
{
"type": "signin",
"title": "Click me for signin",
"value": "https://signin.com"
}
```

# [C#](#tab/csharp)

The following code shows an example of `signin` action type in C#:

```csharp
var button = new CardAction()
{
    Type = ActionTypes.Signin,
    Title = "Click me for signin",
    Value = "https://signin.com"
};
```

# [JavaScript/Node.js](#tab/javascript)

The following code shows an example of `signin` action type in JavaScript:

```javascript
CardFactory.actions([
{
    type: "signin",
    title: "Click me for signin",
    value: "https://signin.com"
}])
```

---

## Adaptive Cards actions

Adaptive Cards support four action types:

* [Action.OpenUrl](http://adaptivecards.io/explorer/Action.OpenUrl.html)
* [Action.Submit](http://adaptivecards.io/explorer/Action.Submit.html)
* [Action.ShowCard](http://adaptivecards.io/explorer/Action.ShowCard.html)
* [Action.Execute](/adaptive-cards/authoring-cards/universal-action-model#actionexecute)

You can also modify the Adaptive Card `Action.Submit` payload to support existing Bot Framework actions using an `msteams` property in the `data` object of `Action.Submit`. The next section provide details on how to use existing Bot Framework actions with Adaptive Cards.

> [!NOTE]
> Adding `msteams` to data with a Bot Framework action does not work with an Adaptive Card task module.

### Adaptive Cards with messageBack action

To include a `messageBack` action with an Adaptive Card include the following details in the `msteams` object:

> [!NOTE]
> You can include additional hidden properties in the `data` object, if required.

| Property | Description |
| --- | --- |
| `type` | Set to `messageBack`. |
| `displayText` | Optional. Used by the user in the chat stream when the action is performed. This text is not sent to your bot. |
| `value` | Sent to your bot when the action is performed. You can encode context for the action, such as unique identifiers or a JSON object. |
| `text` | Sent to your bot when the action is performed. Use this property to simplify bot development. Your code can check a single top-level property to dispatch bot logic. |

The following code shows an example of Adaptive Cards with `messageBack` action:

```json
{
  "type": "Action.Submit",
  "title": "Click me for messageBack",
  "data": {
    "msteams": {
        "type": "messageBack",
        "displayText": "I clicked this button",
        "text": "text to bots",
        "value": "{\"bfKey\": \"bfVal\", \"conflictKey\": \"from value\"}"
    }
  }
}
```

### Adaptive Cards with imBack action

To include an `imBack` action with an Adaptive Card include the following details in the `msteams` object:

> [!NOTE]
> You can include additional hidden properties in the `data` object, if required.

| Property | Description |
| --- | --- |
| `type` | Set to `imBack`. |
| `value` | String that needs to be echoed back in the chat. |

The following code shows an example of Adaptive Cards with `imBack` action:

```json
{
  "type": "Action.Submit",
  "title": "Click me for imBack",
  "data": {
    "msteams": {
        "type": "imBack",
        "value": "Text to reply in chat"
    }
  }
}
```

### Adaptive Cards with signin action

To include a `signin` action with an Adaptive Card include the following details in the `msteams` object:

> [!NOTE]
> You can include additional hidden properties in the `data` object, if required.

| Property | Description |
| --- | --- |
| `type` | Set to `signin`. |
| `value` | Set to the URL where you want to redirect.  |

The following code shows an example of Adaptive Cards with `signin` action:

```json
{
  "type": "Action.Submit",
  "title": "Click me for signin",
  "data": {
    "msteams": {
        "type": "signin",
        "value": "https://signin.com"
    }
  }
}
```

### Adaptive Cards with invoke action

To include an `invoke` action with an Adaptive Card include the following details in the `msteams` object:

> [!NOTE]
> You can include additional hidden properties in the `data` object, if required.

| Property | Description |
| --- | --- |
| `type` | Set to `task/fetch`. |
| `data` | Set the value.  |

The following code shows an example of Adaptive Cards with `invoke` action:

```json
{
  "type": "Action.Submit",
  "title": "submit",
  "data": {
    "msteams": {
        "type": "task/fetch"
    }
  }
}
```

The following code shows an example of Adaptive Cards with `invoke` action with additional payload data:

```json
{
  "type": "Action.Submit",
  "title": "submit"
  "data": {
    "msteams": {
        "type": "task/fetch"
    },
    "Value1": "some value"
  }
}
```
## Next step

> [!div class="nextstepaction"]
> [Universal Actions for Adaptive Cards](../cards/Universal-actions-for-adaptive-cards/Overview.md)

## See also

* [Cards reference](./cards-reference.md)
* [Use task modules from bots](~/task-modules-and-cards/task-modules/task-modules-bots.md)
* [Adaptive cards in bots](../../bots/how-to/conversations/conversation-messages.md#adaptive-cards)
* [Universal Actions for Adaptive Cards](~/task-modules-and-cards/cards/universal-actions-for-adaptive-cards/overview.md)
* [Adaptive cards form completion](~/task-modules-and-cards/cards/task-modules/what-are-cards.md)