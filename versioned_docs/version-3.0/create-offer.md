---
title: "Create offer"
description: "Learn how to use Google Play and App Store offers to attract and keep users engaged in Adapty"
metadataTitle: "How to create offers in Adapty"
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

Adapty allows you to offer discounted pricing to existing or churned subscribers. To use this feature, you need to first [create the offer in App Store Connect](app-store-offers) and/or [create the offer in Play Console](google-play-offers). Once you have the offer ready in the app stores, you can easily add it to Adapty:

1. Open the [**Paywalls and Products**](https://app.adapty.io/products) section from the Adapty main menu, then select the **Products** tab.

   
<Zoom>
  <img src={require('./img/6b9e928-edit_product.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>



2. Find the product to which you want to add an offer and in the **Actions** column, click the **3-dot** button next to the product and select the **Edit** option.
3. In the opened **Edit product** window, click the **Add Offer** button below the **Offers** title.  

   
<Zoom>
  <img src={require('./img/b0e04fe-add_offer.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>



4. In the added new empty line, enter the offer you wish to add to the product.

   Here are the fields for the offer:

   - **Offer name:** Provide a name for the offer to help identify it within Adapty. Use whatever name is convenient for you.
   - **App Store Offer ID:** This is the unique identifier of the offer [you set in the App Store](app-store-products).
   - **Play Store Offer ID:** Similarly, this is the unique identifier of the offer [you set in the Play Store](android-products).
5. Click the **Save** button to save the newly added offers to the product.