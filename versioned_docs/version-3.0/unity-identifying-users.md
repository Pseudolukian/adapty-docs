---
title: "Unity – Identifying Users"
description: ""
metadataTitle: ""
---

import Zoom from 'react-medium-image-zoom';
import 'react-medium-image-zoom/dist/styles.css';

Adapty creates an internal profile id for every user. But if you have your authentification system you should set your own Customer User Id. You can find the users by Customer User Id in [Profiles](profiles-crm), use it in [server-side API](getting-started-with-server-side-api), it will be sent to all integrations.

### Setting Customer User Id after configuration

You can set your user id at any time with `.Identify()` method. The most common cases are after registration/authorization when the user switches from being an anonymous user to an authenticated user.

```swift title="Swift"
Adapty.Identify("YOUR_USER_ID", (error) => {
  if(error == null) {
    // successful identify
  }
});
```

Request parameters:

- **Customer User Id** (required): a string user identifier.

:::warning
Resubmitting of significant user data

In some cases (for example, when a user logs into his account again), Adapty's servers already have information about that user. In such a scenario, Adapty SDK will automatically switch to work with the new user. If, during the anonymous user's existence, you passed some data to it (for example, custom attributes or attributions from third-party networks), you should resubmit that data.

it is also quite important to re-request all paywalls and products after identify, because the data of the new user may be different
:::

### Logging out and logging in

You can logout the user anytime by calling `.Logout()` method:

```swift title="Swift"
Adapty.Logout((error) => {
  if(error == null) {
    // successful logout
  }
});
```

You can then login the user using `.Identify()` method.