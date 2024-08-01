---
title: "Apple App Store credentials"
description: ""
metadataTitle: ""
---

To configure the App Store credentials and ensure optimal functionality of the Adapty iOS SDK, navigate to the [iOS SDK](https://app.adapty.io/settings/ios-sdk) tab within the App Settings page of the Adapty Dashboard. Then, configure the following parameters:


<img
  src={require('./img/3d4087e-CleanShot_2023-06-26_at_13.27.042x.png').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>





| Field | Description |
|-----|-----------|
| **Bundle ID** | Your app bundle ID |
| **In-app purchase API (StoreKit 2)** | [Keys](in-app-purchase-api-storekit-2) to enable secure authentication and validation of in-app purchase transaction history requests. |
| **App Store Server Notifications** | URL that is used to enable [server2server notifications](app-store-server-notifications) from the App Store to monitor and respond to users' subscription status changes |
| **App Store Promotional Offers** | Subscription keys for creating [Promotional offers](app-store-promotional-offers) in Adapty for specific products. |
| **App Store Connect shared secret (LEGACY)** | <p>**Legacy key for StoreKit 1 and Adapty SDK prior to v.2.9.0**</p><p></p><p>[A key](app-store-shared-secret) for receipts validation and preventing fraud in your app.</p> |