# ar_flutter_plugin (Unofficial iOS Firebase-free Fork)

> **⚠️ This is an unofficial fork of [ar_flutter_plugin](https://github.com/CariusLars/ar_flutter_plugin) by CariusLars.**
>
> This fork removes the Firebase and Cloud Anchor dependencies **for iOS only**, making it suitable for publishing apps to the Apple App Store without Firebase-related complications. Android is unaffected and retains full original functionality including Cloud Anchors.
>
> Since this is not a published pub.dev package, you must reference it directly via Git in your `pubspec.yaml` (see [Installing](#installing) below).

---

[![pub package](https://img.shields.io/pub/v/ar_flutter_plugin.svg)](https://pub.dev/packages/ar_flutter_plugin)

Flutter Plugin for Augmented Reality - Supports ARKit for iOS and ARCore for Android devices.

Many thanks to Oleksandr Leuschenko for the [arkit_flutter_plugin](https://github.com/olexale/arkit_flutter_plugin) and to Gian Marco Di Francesco for the [arcore_flutter_plugin](https://github.com/giandifra/arcore_flutter_plugin) which both served as a great basis and starting point for this project.

## What's Different in This Fork

| Feature | Original Plugin | This Fork |
|---|---|---|
| Firebase dependency (iOS) | ✅ Required | ❌ Removed |
| Cloud Anchors (iOS) | ✅ Supported | ❌ Removed |
| Cloud Anchors (Android) | ✅ Supported | ✅ Supported |
| Cloud Anchor example files | ✅ Included | ❌ Removed (caused build issues) |
| App Store publishing | ⚠️ May be blocked by Firebase | ✅ No Firebase, no blockers |
| pub.dev package | ✅ Available | ❌ Git reference only |

**Why this fork exists:** The original plugin includes Firebase as a dependency for its Cloud Anchor functionality. This can prevent apps from being published to the Apple App Store if Firebase is not otherwise needed in your project. This fork strips out Firebase and Cloud Anchor support on the iOS side, leaving the rest of the plugin fully intact.

If you need **Cloud Anchor support on iOS**, use the [original plugin](https://github.com/CariusLars/ar_flutter_plugin) instead.

---

## Getting Started

### Installing

Since this fork is not published on pub.dev, add it to your `pubspec.yaml` as a Git dependency:

```yaml
dependencies:
  ar_flutter_plugin:
    git:
      url: https://github.com/FriedrichWimmer/ar_flutter_plugin.git
      ref: main  # or a specific branch/commit hash
```

Then run:

```bash
flutter pub get
```

### Importing

Add this to your code:

```dart
import 'package:ar_flutter_plugin/ar_flutter_plugin.dart';
```

If you have problems with permissions on iOS (e.g. with the camera view not showing up even though camera access is allowed), add this to the ```podfile``` of your app's ```ios``` directory:

```pod
  post_install do |installer|
    installer.pods_project.targets.each do |target|
      flutter_additional_ios_build_settings(target)
      target.build_configurations.each do |config|
        # Additional configuration options could already be set here

        # BEGINNING OF WHAT YOU SHOULD ADD
        config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
          '$(inherited)',

          ## dart: PermissionGroup.camera
          'PERMISSION_CAMERA=1',

          ## dart: PermissionGroup.photos
          'PERMISSION_PHOTOS=1',

          ## dart: [PermissionGroup.location, PermissionGroup.locationAlways, PermissionGroup.locationWhenInUse]
          'PERMISSION_LOCATION=1',

          ## dart: PermissionGroup.sensors
          'PERMISSION_SENSORS=1',

          ## dart: PermissionGroup.bluetooth
          'PERMISSION_BLUETOOTH=1',

          # add additional permission groups if required
        ]
        # END OF WHAT YOU SHOULD ADD
      end
    end
  end
```

---

### Example Applications

To try out the plugin, it is best to have a look at one of the following examples implemented in the `Example` app.

> **Note:** The Cloud Anchors and External Object Management example files have been **removed entirely** from this fork, as they imported Cloud Anchor functionality that caused build issues. For reference, the originals can be found in the [upstream repository](https://github.com/CariusLars/ar_flutter_plugin).

| Example Name                   | Description                                                                                                                                                                                                                                                                                                                             | Link to Code                                                                                                                                   |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| Debug Options                  | Simple AR scene with toggles to visualize the world origin, feature points and tracked planes                                                                                                                                                                                                                                           | [Debug Options Code](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/debugoptionsexample.dart)                  |
| Local & Online Objects         | AR scene with buttons to place GLTF objects from the flutter asset folders, GLB objects from the internet, or a GLB object from the app's Documents directory at a given position, rotation and scale. Additional buttons allow to modify scale, position and orientation after objects have been placed.                                | [Local & Online Objects Code](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/localandwebobjectsexample.dart)   |
| Objects & Anchors on Planes    | AR Scene in which tapping on a plane creates an anchor with a 3D model attached to it                                                                                                                                                                                                                                                   | [Objects & Anchors on Planes Code](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/objectgesturesexample.dart)  |
| Object Transformation Gestures | Same as Objects & Anchors on Planes example, but objects can be panned and rotated using gestures after being placed                                                                                                                                                                                                                    | [Objects & Anchors on Planes Code](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/objectsonplanesexample.dart) |
| Screenshots                    | Same as Objects & Anchors on Planes Example, but the snapshot function is used to take screenshots of the AR Scene                                                                                                                                                                                                                      | [Screenshots Code](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/screenshotexample.dart)                     |
| ~~Cloud Anchors~~              | ~~AR Scene in which objects can be placed, uploaded and downloaded, creating an interactive AR experience sharable between multiple devices.~~ **Removed — see upstream.**                                                                                                                                                              | [Cloud Anchors Code (upstream)](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/cloudanchorexample.dart)        |
| ~~External Object Management~~ | ~~Similar to the Cloud Anchors example, but contains UI to choose between different models managed via an external Firestore database.~~ **Removed — see upstream.**                                                                                                                                                                    | [External Model Management Code (upstream)](https://github.com/CariusLars/ar_flutter_plugin/blob/main/example/lib/examples/externalmodelmanagementexample.dart) |

---

## Contributing

This is a minimal maintenance fork focused solely on removing Firebase/iOS Cloud Anchor dependencies. For general contributions to the AR Flutter plugin, please direct them to the [original repository](https://github.com/CariusLars/ar_flutter_plugin).

For issues specific to this fork, feel free to open an issue here.

---

## Plugin Architecture

This is a rough sketch of the architecture the plugin implements:

![ar_plugin_architecture](./AR_Plugin_Architecture_highlevel.svg)
