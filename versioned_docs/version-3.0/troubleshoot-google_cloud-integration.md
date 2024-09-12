---
title: "Troubleshoot Google Cloud Storage integration"
description: "Find solutions to common problems that can cause Adapty's automatic export of events or paywall views to Google Cloud to stop working, including bucket deletion, revoked access keys, and expired licenses"
metadataTitle: "Troubleshoot Adapty Integration Issues with Google Cloud"
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

While configuring the automatic export of Adapty events or paywall views, Adapty validates every step to prevent invalid setups. If you encounter an error message, follow the below instructions to resolve the issue:

| Error | Possible reasons and solutions |
|-----|------------------------------|
| <p>403 GET URL: your_service_account_email</p><p>does not have `storage.buckets.get`</p><p>access to the Google Cloud Storage bucket.</p><p>Permission 'storage.buckets.get' denied on</p><p>resource (or it may not exist).</p> | <p>Possible reasons:</p><p></p><p>1. Incorrect **Google Cloud project ID**. Check for typos.</p><p>2. Incorrect **Google Cloud client ID**. Check for typos.</p><p>3. Incorrect role of the service account. Grant correct role when [granting access to the bucket](google-cloud-setup#grant-access-to-google-cloud-storage-bucket).</p><p>4. The **Google Cloud service account private key ID** is incorrect. Check for typos.</p> |
| 404 GET URL: The specified bucket does not exist. | The bucket is not found. Check for typos in the **Google Cloud bucket name** field. |
| <p>('invalid_grant: Invalid grant: account not found',</p><p>'error': 'invalid_grant', 'error_description': 'Invalid grant: account not found')</p> | <p>The service account is not found.</p><p></p><p>1. Check for typos</p><p>2. Make sure the service account belongs to the project you are using for the integration.</p> |
| Could not deserialize key data. | <p>The private key is incorrect.</p><p></p><p>1. Make sure you've replaced allnew line sequences(`\n`) with line breaks.</p><p>2. Check for typos</p> |


However, previously working integrations can sometimes stop functioning. Here are some possible reasons and solutions:

| Reason                                       | Solution                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| :------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The Google Cloud Storage bucket was deleted. | Create it again or [reconfigure the integration](https://www.notion.so/adapty/google-cloud-setup#set-up-google-cloud-storage-integration-in-the-adapty-dashboard) to use another bucket.                                                                                                                                                                                                                                                       |
| The service account was deleted.             | 1. Create a new service account and create a new access key for it as described in the [Create Google Cloud Storage service account](https://www.notion.so/adapty/google-cloud-setup#choose-events-to-send-and-map-event-names)   section. 2. [Reconfigure the integration](https://www.notion.so/adapty/google-cloud-setup#set-up-google-cloud-storage-integration-in-the-adapty-dashboard)  to use the new access key and secret access key. |
| The Access key was revoked.                  | 1. Create a new access key as described in the [Create Google Cloud Storage service account](https://www.notion.so/adapty/google-cloud-setup#choose-events-to-send-and-map-event-names) section. 2. [Reconfigure the integration](https://www.notion.so/adapty/google-cloud-setup#set-up-google-cloud-storage-integration-in-the-adapty-dashboard)  to use the new access key and secret access key.                                           |
| Amazon license was canceled/ expired.        | Purchase a new Google Cloud Storage license.                                                                                                                                                                                                                                                                                                                                                                                                   |