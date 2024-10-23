---
title: "API objects"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

## Objects

Adapty API has JSON objects so you can understand a response structure and wrap it into your code.

All datetime values are [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601), for example, "2020-01-15T15:10:36.517975+0000".

### Profile

Info about the [customer and his subscription.  ](server-side-api-objects#profile)

| Param                    | Type  | Required | Nullable | Description                                                                                                                                                                                                                                         |
| :----------------------- | :---- | :------- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **profile\_id**          | UUID  | ✅        | ❌        | Adapty profile ID                                                                                                                                                                                                                                   |
| **customer\_user\_id**   | str   | ✅        | ✅        | User ID in developer’s \(your\) system.                                                                                                                                                                                                             |
| **paid\_access\_levels** | dict  | ✅        | ✅        | Dictionary where the keys are paid access level identifiers configured by a developer in the Adapty Dashboard. Values are [CustomerAccessLevel](#customeraccesslevel) objects. Can be null if the customer has no access levels                     |
| **subscriptions**        | dict  | ✅        | ✅        | Dictionary where the keys are vendor product IDs. Values are [Subscription](#subscription) objects. Can be null if the customer has no subscriptions                                                                                                |
| **non\_subscriptions**   | dict  | ✅        | ✅        | Dictionary where the keys are vendor product ids. Values are an array of [Non-Subscription](#non-subscription) objects. Can be null if the customer has no purchases.                                                                               |
| **custom\_attributes**   | dict  | ✅        | ✅        | The dictionary that collects the profile's all custom attributes about its own users. A maximum of 10 custom attributes for the profile are allowed to be set. Only strings and floats are allowed as values, booleans will be converted to floats. |
| **total\_revenue\_usd**  | float | ✅        | ❌        | Float value, it is equal to all total revenue USD which earned the profile.                                                                                                                                                                         |

### CustomerAccessLevel

Info about customer’s [access level](access-level).

Configure which products unlock premium \(paid\) features in your app using the Adapty Dashboard. Then, check in the app for the access level available for a user, for example  in the way below \(in Swift \)

```swift title="Swift"
if (profile.paidAccessLevels["premium"]?.isActive == true) {
    /* Grant user access to paid functions of the app */
}
```

| Param                                 | Type          | Required | Nullable | Description                                                  |
| :------------------------------------ | :------------ | :------- | :------- | :----------------------------------------------------------- |
| **id**                                | str           | ✅        | ❌        | Paid Access Level identifier configured by a developer in the Adapty Dashboard |
| **is\_active**                        | bool          | ✅        | ❌        | Boolean indicating whether the paid access level is active   |
| **expires\_at**                       | ISO 8601 date | ✅        | ✅        | The datetime when the access level will expire. It may be in the past and may be null for lifetime access |
| **starts\_at**                        | ISO 8601 date | ✅        | ✅        | The datetime when the access level will be active. May be in the future |
| **is\_lifetime**                      | bool          | ✅        | ❌        | Boolean indicating whether the paid access level is active for a lifetime \(no expiration date\). If set to true you shouldn’t use expires\_at |
| **vendor\_product\_id**               | str           | ✅        | ✅        | <p>This is the product ID from the vendor's system (App Store, Google Play, Stripe, etc.) that unlocked the access level. </p><p>If an access level was granted without a transaction, one of the following values will be returned:</p> <ul>   <li> **adapty_server_side_product**: Access was [granted through the server-side API](server-side-api-specs#prolonggrant-a-subscription-for-a-user).</li>   <li> **adapty_dashboard_product**: Access was [granted in the Adapty Dashboard](give-access-level-to-specific-customer#give-access-level-to-a-specific-customer-in-the-adapty-dashboard).</li>   <li> **adapty_promotion**: legacy option</li> </ul> |
| **base_plan_id**                      | str           | ✅        | ✅        | [Base plan ID](https://support.google.com/googleplay/android-developer/answer/12154973)   in the Google Play Store or [price ID](https://docs.stripe.com/products-prices/how-products-and-prices-work#what-is-a-price)   in Stripe. |
| **store**                             | str           | ✅        | ✅        | Store where the product was purchased. Possible values are: **app\_store**, **play\_store**, and **adapty** |
| **activated\_at**                     | ISO 8601 date | ✅        | ❌        | The datetime when the access level was activated. May be in the future |
| **renewed\_at**                       | ISO 8601 date | ✅        | ✅        | The datetime when the access level was renewed               |
| **will\_renew**                       | bool          | ✅        | ❌        | Boolean indicating whether an auto-renewable subscription is set to renew |
| **is\_in\_grace\_period**             | bool          | ✅        | ❌        | Boolean indicating whether auto-renewable subscription is in the [grace period](https://developer.apple.com/news/?id=09122019c) |
| **unsubscribed\_at**                  | ISO 8601 date | ✅        | ✅        | The datetime when an auto-renewable subscription was cancelled. Subscription can still be active, it means that auto-renewal is turned off. It will be set to null if the user reactivates the subscription |
| **billing\_issue\_detected\_at**      | ISO 8601 date | ✅        | ✅        | The datetime when a billing issue was detected and a vendor was not able to charge the card. Subscription can still be active. Will set to null if the charge is successful |
| **active\_introductory\_offer\_type** | str           | ✅        | ✅        | The type of active [introductory offer](https://developer.apple.com/app-store/subscriptions/#offering-introductory-prices). Possible values are: **free\_trial**, **pay\_as\_you\_go**, and **pay\_up\_front**. If the value is not null it means that the offer was applied during the current subscription period |
| **active\_promotional\_offer\_type**  | str           | ✅        | ✅        | The type of active [promotional offer](https://developer.apple.com/app-store/subscriptions/#subscription-offers). Possible values are: **free\_trial**, **pay\_as\_you\_go**, and **pay\_up\_front**. If the value is not null it means that the offer was applied during the current subscription period |

### Subscription

Info about vendor subscription. You don’t have to use this object in most cases, **CustomerPaidAccessLevel** is the preferred way to work with access to the app. When using this object, you need to implement processing logic for each subscription period \(a week, a month, a year, lifetime\).  

| Param                                 | Type          | Required | Nullable | Description                                                  |
| :------------------------------------ | :------------ | :------- | :------- | :----------------------------------------------------------- |
| **is\_active**                        | bool          | ✅        | ❌        | Boolean indicating whether the subscription is active        |
| **expires\_at**                       | ISO 8601 date | ✅        | ✅        | The datetime when access level will expire. May be in the past and may be null for lifetime access |
| **starts\_at**                        | ISO 8601 date | ✅        | ✅        | The datetime when the subscription will be active. May be in the future for season subscriptions |
| **is\_lifetime**                      | bool          | ✅        | ❌        | Boolean that indicates whether the subscription is active for a lifetime without an expiration date. If set to true you shouldn’t use expires\_at |
| **vendor\_product\_id**               | str           | ✅        | ✅        | <p>This is the product ID from the vendor's system (App Store, Google Play, Stripe, etc.) that unlocked the access level. If an access level was granted without a transaction, one of the following values will be returned:</p> <ul>   <li> **adapty_server_side_product**: Access was granted through the server-side API</li>   <li> **adapty_dashboard_product**: Access was granted for a product the user purchased</li>   <li> **adapty_promotion**: legacy option</li> </ul> |
| **base_plan_id**                      | str           | ✅        | ✅        | [Base plan ID](https://support.google.com/googleplay/android-developer/answer/12154973)  in the Google Play Store or [price ID](https://docs.stripe.com/products-prices/how-products-and-prices-work#what-is-a-price)  in Stripe. |
| **vendor\_transaction\_id**           | str           | ✅        | ✅        | Transaction id in the vendor system                          |
| **vendor\_original\_transaction\_id** | str           | ✅        | ✅        | Original transaction id in vendor system. For auto-renewable subscription, this will be the ID of the first transaction in the subscription |
| **store**                             | str           | ✅        | ✅        | Store where the product was purchased. Possible values are: **app\_store**, **play\_store**, and **adapty** |
| **activated\_at**                     | ISO 8601 date | ✅        | ❌        | The datetime when the access level was activated. May be in the future |
| **renewed\_at**                       | ISO 8601 date | ✅        | ✅        | The datetime when the access level was renewed               |
| **will\_renew**                       | bool          | ✅        | ❌        | Boolean indicating whether an auto-renewable subscription is set to renew. If a user did not cancel a subscription, will be set to true |
| **is\_in\_grace\_period**             | bool          | ✅        | ❌        | Boolean indicating whether an auto-renewable subscription is in the [grace period](https://developer.apple.com/news/?id=09122019c) |
| **unsubscribed\_at**                  | ISO 8601 date | ✅        | ✅        | The datetime when an auto-renewable subscription was canceled. Subscription can still be active, it just means that auto-renewal is turned off. Will be set to null if the user reactivates the subscription |
| **billing\_issue\_detected\_at**      | ISO 8601 date | ✅        | ✅        | The datetime when the billing issue was detected \(vendor was not able to charge the card\). Subscription can still be active. Will be set to null if the charge is successful. |
| **active\_introductory\_offer\_type** | str           | ✅        | ✅        | The type of active [introductory offer](https://developer.apple.com/app-store/subscriptions/#offering-introductory-prices). Possible values are: **free\_trial**, **pay\_as\_you\_go**, and **pay\_up\_front**. If the value is not null it means that the offer was applied during the current subscription period |
| **active\_promotional\_offer\_type**  | str           | ✅        | ✅        | The type of active [promotional offer](https://developer.apple.com/app-store/subscriptions/#subscription-offers). Possible values are: **free\_trial**, **pay\_as\_you\_go**, and **pay\_up\_front**. If the value is not null it means that the offer was applied during the current subscription period |
| **is\_sandbox**                       | bool          | ✅        | ❌        | Boolean indicating whether the product was purchased in the sandbox or production environment |

### Non Subscription

Info about non-subscription purchases. These can be one-time \(consumable\) products, unlocks \(like new map unlock in the game\), etc.  

| Param                                 | Type          | Required | Nullable | Description                                                  |
| :------------------------------------ | :------------ | :------- | :------- | :----------------------------------------------------------- |
| **purchase\_id**                      | str           | ✅        | ❌        | Identifier of the purchase in Adapty. You can use it to ensure that you’ve already processed this purchase, for example tracking one-time products |
| **vendor\_product\_id**               | str           | ✅        | ✅        | <p>This is the product ID from the vendor's system (App Store, Google Play, Stripe, etc.) that unlocked the access level. If an access level was granted without a transaction, one of the following values will be returned:</p> <ul>   <li> **adapty_server_side_product**: Access was granted through the server-side API</li>   <li> **adapty_dashboard_product**: Access was granted for a product the user purchased</li>   <li> **adapty_promotion**: legacy option</li> </ul> |
| **vendor\_transaction\_id**           | str           | ✅        | ✅        | Transaction ID in the vendor system                          |
| **vendor\_original\_transaction\_id** | str           | ✅        | ✅        | Original transaction ID in vendor system. For auto-renewable subscription, this will be the ID of the first transaction in the subscription |
| **store**                             | str           | ✅        | ✅        | Store where the product was purchased. Possible values are **app\_store**, **play\_store**, **adapty** |
| **purchased\_at**                     | ISO 8601 date | ✅        | ❌        | The datetime when the product was purchased                  |
| **is\_one\_time**                     | bool          | ✅        | ❌        | Boolean indicating whether the product should only be processed once. For example, if a user purchased 500 coins in a game. If true, the purchase will be returned by Adapty API one time only |
| **is\_sandbox**                       | bool          | ✅        | ❌        | Boolean indicating whether the product was purchased in a sandbox or production environment. |