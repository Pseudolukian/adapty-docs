---
title: "Flutter – Installation (COPY)"
description: ""
metadataTitle: ""
---

:::danger
Read checklist before releasing the app

Be sure to carefully read [Release Checklist](release-checklist) when releasing your app. It's critical to make things right from the first attempt!
:::

Add Adapty to your `pubspec.yaml` file:

```yaml title="pubspec.yaml"
dependencies:
  adapty_flutter: ^2.10.0
```

And then run:

```bash title="Bash"
flutter pub get
```

After that, you can import Adapty SDK in your application like this:

```dart title="Dart"
import 'package:adapty_flutter/adapty_flutter.dart';
```