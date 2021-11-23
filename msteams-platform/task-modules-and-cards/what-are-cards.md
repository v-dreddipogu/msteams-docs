---
title: Cards
description: Describes cards and how they are used in bots, connectors, and messaging extensions
ms.localizationpriority: medium
keywords: connectors bots cards messaging
ms.topic: overview
---

# Cards

A card is a user interface (UI) container for short or related pieces of information. Cards can have multiple properties and attachments and can include buttons, which trigger [card actions](~/task-modules-and-cards/cards/cards-actions.md). Using cards, you can organize information into groups and give users the opportunity to interact with specific parts of the information.

The bots for Teams support the following types of cards:
 
- Adaptive Card
- Hero card
- List card
- Office 365 Connector card
- Receipt card
- Signin card
- Thumbnail card
- Card collections

You can add rich text formatting to your cards using either Markdown or HTML, depending on the card type. Cards used by bots and messaging extensions in Microsoft Teams, add and respond to these card actions, `openUrl`, `messageBack`, `imBack`, `invoke`, and `signin`.

Teams uses cards in three different places:

* Connectors
* Bots
* Messaging extensions

## Cards in connectors

Cards were first defined as part of Outlook and Office 365 and are now used as part of Office 365 Connectors. Like many Office 365 applications, Teams supports connectors. For more information, see [Office 365 Connectors for Teams](~/webhooks-and-connectors/what-are-webhooks-and-connectors.md). You can find the specification for cards in connectors in [actionable message card reference](/outlook/actionable-messages/card-reference).

## Cards in bots

The Microsoft Bot Framework extends the cards specification by adding a set of predefined cards that bots can use as part of bot messages. Teams supports bots using the Bot Framework, but it supports a different set of these cards. General information on cards in Bot Framework can be found in [add rich card attachments to messages](/bot-framework/nodejs/bot-builder-nodejs-send-rich-cards). These cards are called simple cards in Teams.

Bots in Teams can use simple cards, connector cards, or Adaptive Cards. [Types of cards](~/task-modules-and-cards/cards/cards-reference.md) provides information on cards, supported by bots in Teams.

## Cards in messaging extensions

[Messaging extensions](~/messaging-extensions/what-are-messaging-extensions.md) can also return a card. Messaging extensions can use simple cards, connector cards, or Adaptive Cards. These cards are found in [types of cards](~/task-modules-and-cards/cards/cards-reference.md).

## Types of cards

All cards used by Teams are listed in [types of cards](~/task-modules-and-cards/cards/cards-reference.md). This reference also describes differences between Bot Framework cards and cards in Teams.

## Adaptive Cards

[Adaptive Cards](~/task-modules-and-cards/cards/cards-reference.md#adaptive-card) are a new cross product specification for cards in Microsoft products including bots, Cortana, Outlook, and Windows. They are the recommended card type for new Teams development. For general information from the Adaptive Cards team, see [Adaptive Cards overview](/adaptive-cards). You can use Adaptive Cards anywhere you use existing hero cards, Office 365 cards, and thumbnail cards.

In addition to Adaptive Cards, Teams supports two other types of cards:

* Connector cards: Used as part of Office 365 Connectors.
* Simple cards: Used from the Bot Framework, such as the thumbnail and hero cards.

### Type-ahead search in Adaptive Cards  

Type ahead search added as an input control in Adaptive Cards enable [dynamic search](~/task-modules-and-cards/cards/dynamic-search.md) experience from a dynamically loaded dataset. It also allows users to do a type-ahead static search within a list with limited number of choices. The mobile and desktop clients support type ahead dynamic search experience. 

### Adaptive Cards and Incoming Webhooks

> [!NOTE]
> * All native Adaptive Card schema elements, except `Action.Submit`, are fully supported.
> * The supported actions are [**Action.OpenURL**](https://adaptivecards.io/explorer/Action.OpenUrl.html), [**Action.ShowCard**](https://adaptivecards.io/explorer/Action.ShowCard.html), [**Action.ToggleVisibility**](https://adaptivecards.io/explorer/Action.ToggleVisibility.html), and [**Action.Execute**](/adaptive-cards/authoring-cards/universal-action-model#actionexecute).

Adaptive Cards with Incoming Webhooks enables you to use the rich and flexible capabilities of Adaptive Cards. It sends data using Incoming Webhooks in Teams from their web service.

### Adaptive cards form completion

Adaptive cards form shows a visual informational feedback message when card action completes to a configurable set of card actions.

User can see feedback message after card action is completed

**Error:** When card action completes, user receives feedback as **Something went wrong, Try again** banner if there is an error in the attempt.

Image

**Success:** When card action completes, user receives feedback as **Your response was sent to the app** banner if the attempt is successful.

Image

## Support for AAD Object ID and UPN in user mention 

Bots with Adaptive Cards support user mention IDs, such as AAD Object ID and User Principle Name (UPN) in addition to the existing IDs. Incoming webhooks start to support user mention in Adaptive Card with the AAD Object ID and UPN.

## Next step

> [!div class="nextstepaction"]
> [Types of cards](~/task-modules-and-cards/cards/cards-reference.md)

## See also

* [Format cards in Teams](~/task-modules-and-cards/cards/cards-format.md)
* [Design Adaptive Cards](~/task-modules-and-cards/cards/design-effective-cards.md)
* [Adaptive cards in bots](../bots/how-to/conversations/conversation-messages.md#adaptive-cards)