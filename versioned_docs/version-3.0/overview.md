---
title: "Analytics overview"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

[Overview](https://app.adapty.io/overview) is a section of Adapty Dashboard that allows you to see multiple metrics in a single place. You can customize which charts you would like to see and view data for all of your apps at once (unlike in [Charts](charts) which you can use to dive deeper into a particular app).

It's located right under Home in the left-side menu:


<Zoom>
  <img src={require('./img/016b5bb-CleanShot_2024-01-22_at_18.38.162x.png').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





## Charts

Overview has the following charts available (you can click on the name to learn more about how we calculate it):

- [Revenue](revenue)
- [MRR](mrr)
- [ARR](arr)
- [ARPPU](arppu)
- [New trials](new-trials)
- [New subscriptions](reactivated-subscriptions)
- [Active trials](active-trials)
- [Active subscriptions](active-subscriptions)
- [New non-subscriptions](non-subscriptions)
- [Refund events](refund-events)
- [Refund money](refund-money)

You can customize which charts to show as well as their order. To do that, press Edit in the top-right corner and then either remove charts you don't need, add more or rearrange existing ones by drag and dropping. You can also customize Overview contents in the "Add" menu:


<Zoom>
  <img src={require('./img/d6220fa-CleanShot_2024-01-22_at_20.01.05.gif').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





## Controls

Controls for diving deeper into your data in Overview are very similar to what we have in [Charts](charts) — and most of them are described in [Analytics controls](controls-filters-grouping-compare-proceeds). 

There is one important difference though: you can group and filter by country, store and, most notably, by app — as Overview shows data for all of your apps at once by default. This can be helpful to understand how each of your app contributes to your business metrics:


<Zoom>
  <img src={require('./img/ae3a913-CleanShot_2024-01-22_at_19.56.302x.png').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





:::note
Timezone control

By default, the timezone for Overview is inherited from one of your apps. But if your apps have different reporting timezones in their settings, you can customise the timezone for Overview by pressing Edit and selecting the appropriate one in the dropdown that will appear.
:::