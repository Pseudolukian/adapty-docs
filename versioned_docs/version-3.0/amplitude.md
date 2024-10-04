---
title: "Amplitude"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem'; 

[Amplitude](https://amplitude.com/) is a powerful mobile analytics service. With Adapty, you can easily send events to Amplitude, see how users behave, and then make smart decisions.

Adapty provides a complete set of data that lets you track [subscription events](events) from stores in one place and sends it to your Amplitude account. This allows you to match your user behavior with their payment history in Amplitude, and inform your product decisions.

### How to set up Amplitude integration

To set up the integration with [Amplitude](https://amplitude.com/), go to [Integrations > Amplitude](https://app.adapty.io/integrations/amplitude) in the Adapty Dashboard, turn on a toggle from off to on, and fill out fields.


<Zoom>
  <img src={require('./img/3b50552-CleanShot_2023-08-15_at_16.47.102x.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





You need to enter the **API Key** into Adapty. To find a token, go to your **Project settings** in Amplitude. In case you need help, refer to [official docs](https://amplitude.com/docs/apis/authentication). 


<Zoom>
  <img src={require('./img/2297782-CleanShot_2023-08-15_at_16.53.512x.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





Along with events, Adapty also sends the[ subscription status](subscription-status) and subscription product ID to the [Amplitude user properties.](https://help.amplitude.com/hc/en-us/articles/115002380567#h_39e46c92-7b7f-4358-a96f-c82cc3342e3e)

### Events and tags

Below the credentials, there are three groups of events you can send to Amplitude from Adapty. Simply turn on the ones you need. Check the full list of the events offered by Adapty [here](events).


<Zoom>
  <img src={require('./img/da67694-CleanShot_2023-08-15_at_16.52.352x.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





We recommend using the default event names provided by Adapty. But you can change the event names based on your needs. Adapty will send subscription events to Amplitude using a server-to-server integration, allowing you to view all subscription events in your Amplitude dashboard.

### SDK configuration

Use `Adapty.updateProfile()` method to set `amplitudeDeviceId` or `amplitudeUserId`.  If not set, Adapty uses your user ID (`customerUserId`) or if it's null Adapty ID. Make sure that the user id you use to send data to Amplitude from your app is the same one you send to Adapty.

<Tabs>
<TabItem value="Swift" label="iOS (Swift)" default>
```Text 
import Amplitude 

let builder = AdaptyProfileParameters.Builder()
            .with(amplitudeUserId: Amplitude.instance().userId)
            .with(amplitudeDeviceId: Amplitude.instance().deviceId)

Adapty.updateProfile(params: builder.build())
```
</TabItem>
<TabItem value="kotlin" label="Android (Kotlin)" default>
```kotlin 
//for Amplitude maintenance SDK (obsolete)
val amplitude = Amplitude.getInstance()
val amplitudeDeviceId = amplitude.deviceId
val amplitudeUserId = amplitude.userId

//for actual Amplitude Kotlin SDK
val amplitude = Amplitude(
    Configuration(
        apiKey = AMPLITUDE_API_KEY,
        context = applicationContext
    )
)
val amplitudeDeviceId = amplitude.store.deviceId
val amplitudeUserId = amplitude.store.userId

//

val params = AdaptyProfileParameters.Builder()
    .withAmplitudeDeviceId(amplitudeDeviceId)
    .withAmplitudeUserId(amplitudeUserId)
    .build()
Adapty.updateProfile(params) { error ->
    if (error != null) {
        // handle the error
    }
}
```
</TabItem>
<TabItem value="Flutter" label="Flutter (Dart)" default>
```javascript 
import 'package:amplitude_flutter/amplitude.dart';

final Amplitude amplitude = Amplitude.getInstance(instanceName: "YOUR_INSTANCE_NAME");

final builder = AdaptyProfileParametersBuilder()
     ..setAmplitudeDeviceId(await amplitude.getDeviceId())
     ..setAmplitudeUserId(await amplitude.getUserId());

try {
     await adapty.updateProfile(builder.build());
} on AdaptyError catch (adaptyError) {
     // handle error
} catch (e) {}
```
</TabItem>
<TabItem value="Unity" label="Unity (C#)" default>
```csharp 
var builder = new Adapty.ProfileParameters.Builder();
builder.SetAmplitudeUserId("AMPLITUDE_USER_ID");
builder.SetAmplitudeDeviceId(amplitude.getDeviceId());

Adapty.UpdateProfile(builder.Build(), (error) => {
    // handle error
});
```
</TabItem>
<TabItem value="RN" label="React Native (TS)" default>
```typescript 
import { adapty } from 'react-native-adapty';

try {
  await adapty.updateProfile({
    amplitudeDeviceId: deviceId,
    amplitudeUserId: userId,
  });
} catch (error) {
  // handle `AdaptyError`
}
```
</TabItem>
</Tabs>
