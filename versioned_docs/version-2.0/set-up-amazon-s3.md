---
title: "Set up Amazon S3 integration"
description: "Follow these steps to integrate Amazon S3 with Adapty, including creating an access policy and IAM user in the AWS dashboard, and configuring the integration in the Adapty Dashboard for exporting events and paywall view"
metadataTitle: "Step-by-Step Guide to Setting Up Amazon S3 Integration with Adapty"
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

Adapty [Amazon S3 integration](s3-exports) consists of the following steps:

1. You [create an access policy for Adapty](set-up-amazon-s3#step-1-create-access-policy) in the AWS dashboard.
2. You [create an IAM user for Adapty](set-up-amazon-s3#step-2-create-iam-user) in the AWS dashboard.
3. [You configure the integration](set-up-amazon-s3#set-up-amazon-s3-integration-in-the-adapty-dashboard) in the Adapty Dashboard. Within this step, you can optionally [map Adapty events with your event names](set-up-amazon-s3#choose-events-to-send-and-map-event-names).

## Create Amazon S3 credentials

This guide will help you create the necessary credentials in your AWS Console.

### Step 1\. Create access policy

1. Open the [**Policy** section](https://console.aws.amazon.com/iamv2/home?region=us-east-1#/policies) in your AWS Console.
2. Click the **Create Policy** button.


<Zoom>
  <img src={require('./img/d8d42bb-s3_policies.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





3. In the **Specify permissions** window, switch to the **JSON** tab.


<Zoom>
  <img src={require('./img/15ba00d-s3_specify_permissions.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





4. In the **JSON** tab, paste the following JSON and replace `adapty-s3-integration-test` with your S3 bucket name: 

```json title="Json"
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListObjectsInBucket",
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::adapty-s3-integration-test"
        },
        {
            "Sid": "AllowAllObjectActions",
            "Effect": "Allow",
            "Action": "s3:*Object",
            "Resource": [
                "arn:aws:s3:::adapty-s3-integration-test/*",
                "arn:aws:s3:::adapty-s3-integration-test"
            ]
        },
        {
            "Sid": "AllowBucketLocation",
            "Effect": "Allow",
            "Action": "s3:GetBucketLocation",
            "Resource": "arn:aws:s3:::adapty-s3-integration-test"
        }
    ]
}
```

5. Click the **Next** button.
6. In the **Review and create** window, define the policy name.


<Zoom>
  <img src={require('./img/2ebaf70-s3_review_and_create_policy.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





7. Click the **Create policy** button to finalize the creation process. The **Review and create** window will close and the new policy will show up in the **Policies** list.

### Step 2\. Create IAM user

To enable Adapty to upload raw data reports to your bucket, you will need to provide them with the Access Key ID and Secret Access Key for a user who has write access to the specific bucket. 

To get access key ID and secret access key: 

1. Open the [**Users** section](https://console.aws.amazon.com/iamv2/home#/users) in your AWS Console.
2. Click the **Create user** button.


<Zoom>
  <img src={require('./img/6bd091d-s3_users.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





3. In the **Set permissions** window, select **Add users to group** radio-button.
4. Click the **Create group** button.


<Zoom>
  <img src={require('./img/45a9ebc-s3_set_permissions.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





5. In the **Create user group** window, specify the name of the user group.
6. Find and select the policy you've created in the [Step 1. Create access policy](set-up-amazon-s3#step-1-create-access-policy) section.


<Zoom>
  <img src={require('./img/ea4e51f-s3_create_user_group.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





7. Click the **Create user group** button. The **Create user group** window and the new group will show up in the **User groups** list of the **Set permissions** window.
8. Select the group you've created.


<Zoom>
  <img src={require('./img/e4dac19-s3_set_permissions_choose_group.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





9. Click the **Next** button.
10. In the **Review and create** window, click the **Create user** button.


<Zoom>
  <img src={require('./img/9975a47-s3_review_new_user.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





11. In the **Users** window, click the new user you've created to open the user's details.


<Zoom>
  <img src={require('./img/e774c5a-s3_users_open_user_details.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





12. In the **Access key best practices & alternatives** window, select the **Third-party service** radio-button and the **Confirmation** check-box.


<Zoom>
  <img src={require('./img/2c717b7-s3_access_key_best_practices.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





13. Click the **Next** button.
14. In the **Set description tag** window, click the **Create access key** button.


<Zoom>
  <img src={require('./img/44d1b47-s3_set_description_tag.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





15. In the **Retrieve access keys** window, click the **Download .csv file** button. This file contains the Access key and Secret access key you will need when [setting up Amazon S3 integration in the Adapty Dashboard](set-up-amazon-s3#set-up-amazon-s3-integration-in-the-adapty-dashboard).


<Zoom>
  <img src={require('./img/e65fe45-s3_retrieve_access_keys.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





16. Click the **Done** button.

## Set up Amazon S3 integration in the Adapty Dashboard

In Adapty, you can set up independent exports of subscription events and paywall views. You can save them to a different or the same Amazon bucket. If the same bucket is used, use the same data in the **Events** and **Paywall visits** tabs below.

To set up the Amazon S3 integration in the Adapty Dashboard:

1. Open [**Integrations** -> **Amazon S3**](https://app.adapty.io/integrations/s3) in your Adapty Dashboard. 

   1. Open the **Events** tab  to set up exporting of Adapty subscription events 
   2. the **Paywall visits** tab to set up exporting Adapty paywall views.

   > 📘 Please note that to export views of custom paywalls, i.e. paywalls designed without Paywall Builder, make sure you [track paywall views](present-remote-config-paywalls#track-paywall-view-events) in your mobile app code.



3. Turn on the **Export subscription events to Amazon S3** toggle to initiate the integration.

   
<Zoom>
  <img src={require('./img/133f9f5-s3_adapty_fields.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>



4. Fill out the integration credentials:

| Field                        | Description                                                                                                                                                                                                                                                |
| :--------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Access Key ID**            | A unique identifier that is used to authenticate a user or application's access to an AWS service.  Find this ID in the CSV file you've downloaded while [creating IAM user](set-up-amazon-s3#step-2-create-iam-user).                                 |
| **Secret Access Key**        | A private key that is used in conjunction with the Access Key ID to authenticate a user or application's access to an AWS service. Find this ID in the CSV file you've downloaded while [creating IAM user](set-up-amazon-s3#step-2-create-iam-user) . |
| **S3 Bucket Name **          | A globally unique name that identifies a specific S3 bucket within the AWS cloud. S3 buckets are a simple storage service that allows users to store and retrieve data objects, such as files and images, in the cloud.                                    |
| **Folder Inside the Bucket** | The name of the folder that you want to have inside the selected S3 bucket. You can specify nested directories, e.g. `adapty-events/com.sample-app`. Please note that S3 simulates folders using object key prefixes, which are essentially folder names.  |

4. Choose the events you want to receive and [map their names](set-up-amazon-s3#choose-events-to-send-and-map-event-names).
5. Additional fields and options are not obligatory; use them as needed. 
6. Remember to click the **Save** button to confirm the changes.

## Choose events to send and map event names

To choose the events to export to the Amazon S3 storage:

1. Open [**Integrations** -> **Amazon S3** -> **Events** tab](https://app.adapty.io/integrations/s3) in your Adapty Dashboard.
2. Choose the [events](events) you want to receive in your server by enabling the toggle next to it in the **Event names** section below the credentials. 
3. If your event names differ from those used in Adapty and you need to keep your names as is, you can set up the mapping by replacing the default Adapty event names with your own.


<Zoom>
  <img src={require('./img/fd5ccb9-CleanShot_2023-08-17_at_14.49.282x.webp').default}
  style={{
    border: '1px solid #727272', /* border width and color */
    width: '700px', /* image width */
    display: 'block', /* for alignment */
    margin: '0 auto' /* center alignment */
  }}
/>
</Zoom>





The event name can be any string. You cannot leave the fields empty for enabled events. If you accidentally removed Adapty event name, you can always copy the name from the [Events to send to 3d-party integrations](events) topic.