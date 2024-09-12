---
title: "Create offer in Play Store"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

:::note
Please check a detailed [Google Play offers](google-play-offers) guide to configure them properly.
:::

You have the ability to create one or several offers for your base plans, and each offer can include up to three phases. These offers can be designed as free trials, introductory pricing, or a combination of both. For more details about setting up base plans and offers, please refer to the official [Google documentation](https://support.google.com/googleplay/android-developer/answer/12154973?hl=en).

It's important to note that in the case of Google’s new billing system, trials won't be automatically assigned to users. To enable this functionality, you must create an offer and specify it during the payment configuration setup.


<Zoom>
  <img src={require('./img/56a2ea9-CleanShot_2023-07-20_at_17.25.042x.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





Previous versions of the Adapty SDK do not support Google's latest subscription features, such as multiple offers per base plan. Only offers marked as **Backwards compatible** in the Google Play Console can be utilized with these SDK versions. It's important to note that only one offer per base plan can be marked as backwards compatible.

You can also learn how to configure products in the Google Play Console by checking our [documentation](android-products).