---
title: "How to get spends data from FB Business Manager"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

We use data from FB Business Manager to build cohort analytics based on your spending, so you can optimize your expenses. Currently, we need data formed in a special way. This page is a tutorial for creating custom data reports in Facebook Business Manager.

### 1. Set up Ads Manager

First of all, you have to enter your <a href="https://business.facebook.com/"> Facebook Business Manager </a>. You may require administrator permissions for this.  
The next step is to open Ads Manager. Usually, it is present in the left panel of your Facebook Business Manager. If you don\`t see the Ads Manager tab there, you can manually add it.


<Zoom>
  <img src={require('./img/1734111-edit_tools.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>






<Zoom>
  <img src={require('./img/01e2f0f-setup_ads_manager.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





### 2. Create custom report

In the Ads Manager tab, you should select the project you are creating the report for and create Custom Report.


<Zoom>
  <img src={require('./img/80e4cd5-general_report_view.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>






<Zoom>
  <img src={require('./img/6bd82d8-create_custom_report.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





#### 2.1 Adjust breakdowns

The next step is to adjust breakdowns. Adapty needs your data granulated by day, AdID, AdSetId, CampaignID. Please see the required breakdowns on a picture below.


<Zoom>
  <img src={require('./img/5281d00-breakdowns.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





#### 2.2 Set up custom metrics

After that, we have to choose the metrics needed for Adapty to perform its analytics. All metrics are grouped by different categories: Performance, Engagement, and Settings. To form a report you need to select the following metrics as shown in the picture below. 

Performance: Reach, Impressions, Amount spent, Clicks (all), CPC (all), CTR (all), Gross impressions (includes invalid impressions for non-human traffic), Attribution setting, Cost per result, Cost per 1000 people reached, CPM (cost per 1000 impressions), Ad Delivery, Ad Set Delivery.

Engagement: Link clicks, Unique link clicks, Outbound clicks, Unique outbound clicks, CTR (link click-trough-rate), Unique CTR (link click-through rate), Outbound CTR (click-through-rate), Unique outbound CTR (click-through-rate), Unique clicks (all), Unique CTR (all), CPC (cost per link click), Cost per unique link click, Cost per outbound click, Cost per unique outbound click, Cost per unique click (all). 

Settings metrics: Reporting starts, Reporting ends, Ad ID, Ad set ID, Ad Set Name, Campaign ID, Campaign name.  


<Zoom>
  <img src={require('./img/a8df062-performance_metrics.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>






<Zoom>
  <img src={require('./img/6df97d7-engagement_metrics.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>






<Zoom>
  <img src={require('./img/e9547ad-settings_metrics.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





You may choose any metrics additional metrics you want, but Adapty won\`t process them for now.  
The same metrics are needed for all other languages. Currently supported languages are English, Russian, German, Turkish, Spanish, Portuguese, and French.

### 3. Download custom report

Then, we have to download the report. Please select CSV file format and don\`t add any summary rows to the file.


<Zoom>
  <img src={require('./img/7f42174-select_download.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>






<Zoom>
  <img src={require('./img/b0e2ad2-finish_export.png').default}
  style={{
    border: 'none', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





### 4. Send report to Adapty

After downloading your report please send it to <a href="mailto:support@adapty.io">support@adapty.io</a> and get your insights right away!