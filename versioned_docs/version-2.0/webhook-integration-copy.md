---
title: "Webhook integration (COPY)"
description: ""
metadataTitle: ""
---

A webhook is a convenient and fast way to get notifications about [events](events). For example, if a new trial is started, a subscription is renewed, or a billing issue happens. The webhook mechanism works through a callback function. You set up a URL (called Endpoint URL) to which an HTTP request is sent when an event occurs. 


<img
  src={require('./img/e5dce30-image_3.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>





Unlike API requests, which require repeatedly requesting information from the server, a webhook is configured only once. When the target event occurs in your app, it immediately sends data via the HTTP protocol.

With webhooks integrated, you can:

- Keep track of subscriptions and purchases in your backend system.
- Automate processes and workflows based on subscription lifecycles.
- Engage with subscribers by reminding them of app benefits, addressing unsubscribe decisions, and handling billing issues.
- Conduct a detailed analysis of the user behavior.

**Integration characteristics**

| Integration characteristic | Description                                                 |
| :------------------------- | :---------------------------------------------------------- |
| Schedule                   | Real-time updates                                           |
| Data direction             | One-way data transmission: from Adapty to your server       |
| Adapty integration flow    | Events are sent by the Adapty server once they are received |

## Events sent to web hook

| Event Name | Description |
|----------|-----------|
| **subscription_started** | A user has activated a subscription without a trial period i.e. he was billed instantly. |
| **subscription_renewed** | A subscription was renewed and the user was charged. For both trial and non-trial subscriptions, this event is sent starting from the second billing. |
| **subscription_expired** | A user has canceled a subscription and it is completely finished, that is  the user has no longer access to the premium features of the app. When the user unsubscribes, subscription_renewal_cancelled event is sent. |
| **trial_started** | A user has activated a trial subscription. |
| **trial_converted** | A trial period has ended and the user was billed, i.e. first purchase was made. |
| **trial_expired** | <p>A trial has expired without converting to a subscription and the user has no longer access to the premium features of the app.</p><p>When the user cancels a trial, trial_renewal_cancelled event is sent.</p> |
| **non_subscription_purchase** | Any non-subscription purchase e.g. lifetime access or consumable product such as coins. |
| **billing_issue_detected** | An attempt to charge the user was made, but a billing issue happened. Usually, it means the user doesn't have enough card balance. |
| **entered_grace_period** | The payment was not successful and the user entered into a grace period. The user still has access to the premium features of your app until the grace period is finished. |
| **trial_renewal_cancelled** | A user turned off subscription auto-renewal during the trial. A user still has access to the premium features of your app until the end of the trial period. |
| **trial_renewal_reactivated** | A user turned on subscription auto-renewal during the trial period. |
| **subscription_renewal_cancelled** | A user turned off subscription auto-renewal. A user still has access to the premium features of your app until the end of the subscription period. |
| **subscription_renewal_reactivated** | A user turned on subscription auto-renewal. |
| **subscription_refunded** | A subscription was refunded \(e.g. by Apple support\). |
| **non_subscription_purchase_refunded** | Non-subscription purchase was refunded. |
| **subscription_paused** | User activated [subscription pause](https://developer.android.com/google/play/billing/subs#pause) (Android only). |
| **subscription_deferred** | A user's subscription has been deferred, ie they were granted free usage time (Android only). Usually, it happens in response to an [API](https://developers.google.com/android-publisher/api-ref/rest/v3/purchases.subscriptions/defer) call from your servers. |
| **access_level_updated** | User's access level updated. |


:::note
**SUBSCRIPTION\_EXPIRED**(previously **SUBSCRIPTION\_CANCELED**) event means that the subscription completely finished and the user has no longer access to the premium features of the app. When the user unsubscribes, **AUTO\_RENEW\_OFF** or **AUTO\_RENEW\_OFF\_SUBSCRIPTION** is sent. The same logic applied to **TRIAL\_CANCELLED.**
:::

## Webhook event structure

Adapty will send you those events you've chosen in the **Events names** section of the [**Integrations** ->  **Webhooks**](https://app.adapty.io/integrations/customwebhook) page.

Each event except for the [access_level_update](webhook#event-access-level-updated) is wrapped into the structure before. Please pay attention that some properties can be absent, if a property field is blank, Adapty does not sent it to webhook.

```json title="Json"
{
  "profile_id": "772204ce-ebf6-4ed9-82b0-d8688ab62b01",
  "customer_user_id": "john.doe",
  "idfv": "00000000-0000-0000-0000-000000000000",
  "idfa": "00000000-0000-0000-0000-000000000000",
  "advertising_id": "00000000-0000-0000-0000-000000000000",
  "profile_install_datetime": "2020-02-18T18:40:22.000000+0000",
  "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_4_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148",
  "email": "john.doe@company.com",
  "event_type": "non_subscription_purchase",
  "event_datetime": "2023-02-18T18:40:22.000000+0000",
  "event_properties": <event-specific properties>,
  "event_api_version": 1,
  "attributions": {"attribution_source1": <attribution_data>, "attribution_source2": <attribution_data>, ...},
  "user_attributes": {"attribute_name1": "attribute_value1", "attribute_name2": "attribute_value2", ...}
  "integration_ids": {"firebase_app_instance_id": "val1", "branch_id": "val2", "one_signal_player_id": "val3", ... }
}
```

### Event parameters

| Property                     | Type                 | Description                                                                                                                                                                                                                                                                                                 |
| :--------------------------- | :------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **profile_id**               | String               | The Сustomer user ID of the profile in Adapty.                                                                                                                                                                                                                                                              |
| **customer_user_id**         | String               | User ID you use in your app to identify the user. For example, it can be your user UUID, email, or any other ID. Null if you didn't set it.                                                                                                                                                                 |
| **idfv**                     | String               | The identifier for vendors (IDFV) is a unique code assigned to all apps developed by a single developer, which in this case refers to your apps                                                                                                                                                             |
| **idfa**                     | String               | The identifier for advertisers (IDFA) is a random device identifier assigned by Apple to a user's device.                                                                                                                                                                                                   |
| **advertising_id**           | String               | The Advertising ID is a unique code assigned by the Android Operating System that advertisers might use to uniquely identify a user's device.                                                                                                                                                               |
| **profile_install_datetime** | ISO 8601 date & time | Installation date and time in format [IOS 8601](https://www.iso.org/iso-8601-date-and-time-format.html): starting with the year, followed by the month, the day, the hour, the minutes, seconds, and milliseconds. For example, 2020-07-10T15:00:00.000000+0000, represents the 10th of July 2020 at 3 p.m. |
| **user_agent**               | String               | User-agent used by the browser on the device.                                                                                                                                                                                                                                                               |
| **email**                    | String               | E-mail of your user.                                                                                                                                                                                                                                                                                        |
| **event_type**               | String               | Event name as set up in the in the **Events names** section of the [**Integrations** ->  **Webhooks**](https://app.adapty.io/integrations/customwebhook)  page in lowercase.                                                                                                                                |
| **event_datetime**           | ISO 8601 date & time | Event date and time in format [IOS 8601](https://www.iso.org/iso-8601-date-and-time-format.html) : starting with the year, followed by the month, the day, the hour, the minutes, seconds, and milliseconds. For example, 2020-07-10T15:00:00.000000+0000, represents the 10th of July 2020 at 3 p.m.       |
| **event_properties**         | JSON                 | JSON of [event properties](events#properties).                                                                                                                                                                                                                                                          |
| **event_api_version**        | Integer              | Adapty API version. The current value is `1`.                                                                                                                                                                                                                                                               |
| **attributions**             | JSON                 | JSON of [attribution data](webhook#attribution-data).                                                                                                                                                                                                                                                   |
| **user_attributes**          | JSON                 | JSON of [custom user attributes](setting-user-attributes#custom-user-attributes).                                                                                                                                                                                                                       |
| **integration_ids**          | JSON                 | JSON of user integration identifiers. If a user doesn't have any identifier or integrations are disabled, then a null is sent.                                                                                                                                                                              |

:::warning
Note that this structure may grow over time — with new data being introduced by us or by the 3rd parties we work with. Make sure that your code that processes it is robust enough and relies on the specific fields, but not on the structure as a whole.
:::

Webhook integration enables the control of sending attribution and user attributes. 

- Enable the **Send Attribution** option in the [**Integrations** ->  **Webhooks**](https://app.adapty.io/integrations/customwebhook) page to send the information about the source of app installs from data providers. 
- Enable the **Send User Attributes** option to send custom user attributes set up from the Adapty SDK, such as user preferences and app usage data.

### Event data for all events except for Access level updated

Here is the full list od event properties that can be sent with an event to webhook. Please pay attention that some properties can be absent.

| Property | Type | Description |
|--------|----|-----------|
| **profile_id** | uuid | Adapty user ID. |
| **currency** | str | Local currency (defaults to USD). |
| **price_usd** | float | Product price before Apple/Google cut. Revenue. |
| **proceeds_usd** | float | Product price after Apple/Google cut. Net revenue. |
| **net_revenue_usd** | float | Net revenue (income after Apple/Google cut and taxes) in USD. Can be empty. |
| **price_local** | float | Product price before Apple/Google cut in local currency. Revenue. |
| **proceeds_local** | float | Product price after Apple/Google cut in local currency. Net revenue. |
| **transaction_id** | str | A unique identifier for a transaction such as a purchase or renewal. |
| **original_transaction_id** | str | The transaction identifier of the original purchase. |
| **purchase_date** | ISO 8601 date | The date and time of product purchase. |
| **original_purchase_date** | ISO 8601 date | The date and time of the original purchase. |
| **environment** | str | Can be _Sandbox_ or _Production_. |
| **vendor_product_id** | str | Product ID in the Apple App Store, Google Play Store, or Stripe. |
| **base_plan_id** | str | [Base plan ID](https://support.google.com/googleplay/android-developer/answer/12154973)   in the Google Play Store or [price ID](https://docs.stripe.com/products-prices/how-products-and-prices-work#what-is-a-price)   in Stripe. |
| **event_datetime** | ISO 8601 date | The date and time of the event. |
| **store** | str | Can be _app_store_ or _play_store_. |
| **trial_duration** | str | Duration of a trial period in days. Sent in a format "{} days" , for example, "7 days". |
| **cancellation_reason** | str | <p>A reason why the user canceled a subscription.</p><p></p><p>Can be</p><p>iOS & Android</p><p>_voluntarily_cancelled_, _billing_error_, _refund_</p><p>iOS</p><p>_price_increase_, _product_was_not_available_, _unknown_</p><p>Android</p><p>_new_subscription_replace_, _cancelled_by_developer_</p> |
| **subscription_expires_at** | ISO 8601 date | The Expiration date of subscription. Usually in the future. |
| **consecutive_payments** | int | The number of periods, that a user is subscribed to without interruptions. Includes the current period. |
| **rate_after_first_year** | bool | Boolean indicates that a vendor reduces cuts to 15%. Apple and Google have 30% first-year cut and 15% after it. |
| **promotional_offer_id** | str | ID of promotional offer as indicated in the Product section of the Adapty Dashboard |
| **store_offer_category** | str | Can be _introductory_ or _promotional_. |
| **store_offer_discount_type** | str | Can be _free_trial_, _pay_as_you_go_ or _pay_up_front_. |
| **paywall_name** | str | Name of the paywall where the transaction originated. |
| **paywall_revision** | int | Revision of the paywall where the transaction originated. The value is set to 1. |
| **developer_id** | str | Developer (SDK) ID of the placement where the transaction originated. |
| **ab_test_name** | str | Name of the A/B test where the transaction originated. |
| **ab_test_revision** | int | Revision of the A/B test where the transaction originated. The value is set to 1. |
| **cohort_name** | str | Name of the audience to which the profile belongs to. |
| **profile_event_id** | uuid | Unique event ID that can be used for deduplication. |
| **store_country** | str | The country sent to us by the store. |
| **profile_ip_address** | str | Profile IP (can be IPv4 or IPv6, with IPv4 preferred when available). It is updated each time IP of the device changes. |
| **profile_country** | str | Determined by Adapty, based on profile IP. |
| **profile_total_revenue_usd** | float | Total revenue for the profile, refunds included. |
| **variation_id** | uuid | Unique ID of the paywall where the purchase was made. |


### Event data for event Access level updated

Access level updated event has a wider list of event properties:

| Property | Type | Description |
|--------|----|-----------|
| **profile_id** | uuid | Adapty user ID. |
| **currency** | str | Local currency (defaults to USD). |
| **price_usd** | float | Product price before Apple/Google cut. Revenue. |
| **proceeds_usd** | float | Product price after Apple/Google cut. Net revenue. |
| **net_revenue_usd** | float | Net revenue (income after Apple/Google cut and taxes) in USD. Can be empty. |
| **price_local** | float | Product price before Apple/Google cut in local currency. Revenue. |
| **proceeds_local** | float | Product price after Apple/Google cut in local currency. Net revenue. |
| **transaction_id** | str | A unique identifier for a transaction such as a purchase or renewal. |
| **original_transaction_id** | str | The transaction identifier of the original purchase. |
| **purchase_date** | ISO 8601 date | The date and time of product purchase. |
| **original_purchase_date** | ISO 8601 date | The date and time of the original purchase. |
| **environment** | str | Can be _Sandbox_ or _Production_. |
| **vendor_product_id** | str | Product ID in the Apple App Store, Google Play Store, or Stripe. |
| **base_plan_id** | str | [Base plan ID](https://support.google.com/googleplay/android-developer/answer/12154973)   in the Google Play Store or [price ID](https://docs.stripe.com/products-prices/how-products-and-prices-work#what-is-a-price)   in Stripe. |
| **event_datetime** | ISO 8601 date | The date and time of the event. |
| **store** | str | Can be _app_store_ or _play_store_. |
| **trial_duration** | str | Duration of a trial period in days. Sent in a format "{} days" , for example, "7 days". |
| **cancellation_reason** | str | <p>A reason why the user canceled a subscription.</p><p></p><p>Can be</p><p>iOS & Android</p><p>_voluntarily_cancelled_, _billing_error_, _refund_</p><p>iOS</p><p>_price_increase_, _product_was_not_available_, _unknown_</p><p>Android</p><p>_new_subscription_replace_, _cancelled_by_developer_</p> |
| **subscription_expires_at** | ISO 8601 date | The Expiration date of subscription. Usually in the future. |
| **consecutive_payments** | int | The number of periods, that a user is subscribed to without interruptions. Includes the current period. |
| **rate_after_first_year** | bool | Boolean indicates that a vendor reduces cuts to 15%. Apple and Google have 30% first-year cut and 15% after it. |
| **promotional_offer_id** | str | ID of promotional offer as indicated in the Product section of the Adapty Dashboard |
| **store_offer_category** | str | Can be _introductory_ or _promotional_. |
| **store_offer_discount_type** | str | Can be _free_trial_, _pay_as_you_go_ or _pay_up_front_. |
| **paywall_name** | str | Name of the paywall where the transaction originated. |
| **paywall_revision** | int | Revision of the paywall where the transaction originated. The value is set to 1. |
| **developer_id** | str | Developer (SDK) ID of the placement where the transaction originated. |
| **ab_test_name** | str | Name of the A/B test where the transaction originated. |
| **ab_test_revision** | int | Revision of the A/B test where the transaction originated. The value is set to 1. |
| **cohort_name** | str | Name of the audience to which the profile belongs to. |
| **profile_event_id** | uuid | Unique event ID that can be used for deduplication. |
| **store_country** | str | The country sent to us by the store. |
| **profile_ip_address** | str | Profile IP (can be IPv4 or IPv6, with IPv4 preferred when available). It is updated each time IP of the device changes. |
| **profile_country** | str | Determined by Adapty, based on profile IP. |
| **profile_total_revenue_usd** | float | Total revenue for the profile, refunds included. |
| **variation_id** | uuid | Unique ID of the paywall where the purchase was made. |
| **access_level_id** | str | Paid access level ID |
| **is_active** | bool | Boolean indicating whether paid access level is active for the profile. |
| **will_renew** | bool | Boolean indicating whether paid access level will be renewed. |
| **is_refund** | bool | Boolean indicating whether transaction is refunded. |
| **is_lifetime** | bool | Boolean indicating whether paid access level is lifetime. |
| **is_in_grace_period** | bool | Boolean indicating whether profile is in grace period. |
| **starts_at** | ISO 8601 date | Date and time when paid access level starts for the user. |
| **renewed_at** | ISO 8601 date | Date and time when paid access will be renewed. |
| **expires_at** | ISO 8601 date | Date and time when paid access will expire. |
| **activated_at** | ISO 8601 date | Date and time when paid access was activated. |
| **billing_issue_detected_at** | ISO 8601 date | Date and time of billing issue. |


### Attribution data

If you've chosen to send attribution data, the following data will be sent with the event:

| Field name          | Field type    | Description                                        |
| :------------------ | :------------ | :------------------------------------------------- |
| **network_user_id** | str           | ID assigned to the user by the attribution source. |
| **status**          | str           | Can be `organic`, `non_organic` or `unknown`.      |
| **created_at**      | ISO 8601 date | Date and time of attribution record creation.      |
| **channel**         | str           | Marketing channel name.                            |
| **campaign**        | str           | Marketing campaign name.                           |
| **ad_group**        | str           | Attribution ad group.                              |
| **ad_set**          | str           | Attribution ad set.                                |
| **creative**        | str           | Attribution creative keyword.                      |

## Access level updated event

Adapty has a special event `access_level_updated`. It is sent only to webhook integration every time the access level is updated/set for a specific customer. Use this event to update a customer's subscription in your database/system. No matter what was the source of access level changes, you will always receive a dedicated event for that, therefore it's more precise and has more details than `subscription_renewed`, `trial_started`, `entered_grace_period`, etc.


<img
  src={require('./img/6375cb2-CleanShot_2022-05-03_at_14.22.56.png').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>





| Property | Type | Description |
|--------|----|-----------|
| **store** | str | Could be app_store, play_store, or stripe. |
| **currency** | str | The 3-letter currency code (ISO-4217) of the transaction. |
| **is_active** | bool | Boolean indicating whether paid access level is active for the profile. |
| **is_refund** | bool | Boolean indicating whether transaction is refunded. |
| **expires_at** | ISO 8601 date | Date and time when paid access will expire. |
| **starts at** | ISO 8601 date | Date and time when paid access level starts for the user. |
| **profile_id** | str | Adapty user ID. |
| **renewed_at** | ISO 8601 date | Date and time when paid access will be renewed. |
| **will_renew** | bool | Boolean indicating whether paid access level will be renewed. |
| **environment** | str | <p>Indicates whether the user is operating in a sandbox or production environment.</p><p></p><p>Values are either Sandbox or Production.</p> |
| **is_lifetime** | bool | Boolean indicating whether paid access level is lifetime. |
| **activated_at** | ISO 8601 date | Date and time when paid access was activated. |
| **purchase_date** | ISO 8601 date | <p>Contains the date of the last transaction (original purchase or renewal).</p><p></p><p>Value format is:</p><p>year-month dayThour:minute:second</p><p>e.g., 2023-02-10T17:22:03.000000+0000</p> |
| **store_country** | str | The country sent to Adapty by the store. |
| **event_datetime** | ISO 8601 date | The date and time of the event. |
| **transaction_id** | str | A unique identifier for a transaction such as a purchase or renewal. |
| **access_level_id** | str | Paid access level ID |
| **profile_country** | str | Profile Country determined by Apple/Google store. |
| **profile_event_id** | str | Unique event ID that can be used for deduplication. |
| **vendor_product_id** | str | <p>Contains the value of Product Id in Apple/Google store.</p><p></p><p>e.g., org.locals.12345</p> |
| **is_in_grace_period** | bool | Boolean indicating whether profile is in grace period. |
| **original_purchase_date** | ISO 8601 date | The date and time of the original purchase. |
| **original_transaction_id** | str | The transaction identifier of the original purchase. |
| **subscription_expires_at** | ISO 8601 date | <p>Contains the expiration date of the latest subscription.</p><p></p><p>Value format is:</p><p>year-month dayThour:minute:second</p><p>e.g., 2023-02-10T17:22:03.000000+0000</p> |
| **profile_total_revenue_usd** | float | Total revenue for the profile, refunds included. |
| **billing_issue_detected_at** | ISO 8601 date | Date and time of billing issue. |
| **cancellation_reason** | str | <p>A reason why the user canceled a subscription.</p><p></p><p>Can be</p><p>**iOS & Android**:</p><p></p><p>- voluntarily_cancelled</p><p>- billing_error</p><p>- refund</p><p>**iOS**:</p><p>- price_increase</p><p>- product_was_not_available</p><p>- unknown</p><p>**Android**:</p><p>- new_subscription_replace</p><p>- cancelled_by_developer</p> |
| **active_introductory_offer_type** | str | Type of the active introductory offer. |
| **active_promotional_offer_type** | str | Type of the active promotional offer. |
| **active_promotional_offer_id** | str | ID of the active promotional offer as indicated in the Product section of the Adapty Dashboard |


Adapty doesn't send `access_level_updated` upon subscription expiration - please, refer to **expires_at** value to end the subscriptions on your side. 

Please note that some properties can only be set using the grant Server-Side API method. You can find more information about this method in the [Adapty Server-Side API documentation.](server-side-api-specs#prolonggrant-a-subscription-for-a-user)

For detailed descriptions of the mentioned properties, you can refer to the[ API objects documentation](server-side-api-objects).