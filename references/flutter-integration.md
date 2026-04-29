# Flutter Platform - Integration Guide & Usage Documentation

## 1. Development Environment Requirements

- **Flutter**: 3.16.0 and above
- **Android**:
  - Android Studio 3.5+
  - Android SDK API Level 19+
  - Android 4.4+ system, supporting armeabi-v7a and arm-v8a architectures
- **iOS**:
  - Xcode 11.0+
  - iOS 9.0+ iPhone or iPad physical device (simulator not supported)
  - Valid developer signing configured

---

## 2. SDK Integration

The Flutter plugin is hosted on GitHub. Project name: **EffectPlayer Flutter**.

### Add Dependency in `pubspec.yaml`

**Integrate the latest version**:
```yaml
dependencies:
  flutter_effect_player:
    git:
      url: https://github.com/Tencent-RTC/EffectPlayer_Flutter.git
```

**Integrate a specific version**:
```yaml
dependencies:
  flutter_effect_player:
    git:
      url: https://github.com/Tencent-RTC/EffectPlayer_Flutter.git
      ref: release_example_tag  # Replace with a specific tag
```

### Dependency Management Commands

```bash
# Get dependencies
flutter pub get

# Upgrade dependencies
flutter pub upgrade
```

---

## 3. Native Configuration (Encrypted Video Playback)

If the animation was converted using TepTools with `Anim Encrypt` enabled, additional configuration is required.

### Android

1. Create `res/xml/network_security_config.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">localhost</domain>
        <domain includeSubdomains="true">127.0.0.1</domain>
    </domain-config>
</network-security-config>
```

2. Reference it in the `<application>` tag of `AndroidManifest.xml`:
```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ... >
```

### iOS

Add to `Info.plist`:
```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

---

## 4. License Initialization

```dart
import 'package:flutter_effect_player/flutter_effect_player.dart';

// Set License
FTCMediaXBase.instance.setLicense(
  LICENSE_URL,
  LICENSE_KEY,
  (errCode, msg) {
    if (errCode == 0) {
      // Authorization successful
    } else {
      // Authorization failed, handle error
    }
  },
);
```

**Key Error Codes**:
| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid parameter |
| -3 | Download failed (network issue) |
| 3015 | Bundle Id / Package Name mismatch |
| 3018 | Authorization file expired |

---

## 5. Log Management

Logging is enabled by default.
- **Android Path**: `/sdcard/Android/data/${your_packagename}/files/TCMediaLog`
- **iOS Path**: Sandbox `Documents/TCMedialog` folder

---

## 6. Player Usage

### 1. Import the View

Integrate the animation playback view using the `FTCEffectAnimView` component:

```dart
FTCEffectAnimView(
  controllerCallback: (controller) {
    _controller = controller;
    // Once the controller is obtained, playback operations can be performed
  },
)
```

### 2. Set the Playback Listener

```dart
_controller.setPlayListener(
  onPlayStart: () {
    // Animation started playing
  },
  onPlayEnd: () {
    // Animation finished playing
  },
  onPlayEvent: (event, param) {
    // Events during playback
  },
  onPlayError: (errorCode) {
    // Playback error
  },
);
```

### 3. Playback Configuration (FTCEffectConfig)

```dart
FTCEffectConfig config = FTCEffectConfig();
config.codecType = FTCEffectCodecType.TC_MPLAYER; // Default engine
config.freezeFrame = FTCEffectFreezeFrame.LAST;   // Stay on the last frame
_controller.setConfig(config);
```

**codecType values**:
| Value | Description |
|-------|-------------|
| `TC_MPLAYER` | Default MPLAYER engine |
| `TC_MCODEC` | MCODEC engine (Android only) |
| `TX_LITEAV_SDK` | Tencent Cloud Player SDK (requires additional import and authorization) |

### 4. Playback Control

```dart
// Start playback (only supports local resources)
_controller.startPlay(localPath);

// Pause
_controller.pause();

// Resume
_controller.resume();

// Loop playback
_controller.setLoop(true);

// Mute
_controller.setMute(true);

// Stop playback
_controller.stopPlay();
```

> **Note**: The player only supports local sdcard video resources. Network resources need to be downloaded locally first, retaining the original file suffix (e.g., .mp4).

### 5. Fusion Animation Configuration

Implement `FResourceFetcher` to replace images or text in the animation:

```dart
_controller.setFetchResource(
  imgFetcher: (resource) async {
    // Return image data of type Uint8List
    return imageBytes;
  },
  textFetcher: (resource) async {
    // Return an FTCEffectText object
    FTCEffectText text = FTCEffectText();
    text.text = "Replacement Text";
    text.color = 0xFFFFFFFF;
    return text;
  },
);
```

---

## 7. FAQ

### License checked failed
Check whether the Effect Player License has been applied for and correctly initialized.

### Error Code -10007
The animation video encoding format is H.265, which is not supported on the current device.

### Error Code -10009
Invalid License.

### Error Code -10012
Missing required dependency (e.g., TX_LITEAV_SDK is used but not imported).

---

## 8. Demo Project

### Environment Preparation
- Flutter 3.16.0+, Android Studio 3.5+ / Xcode 11.0+

### Steps to Run
1. Download the Flutter Demo project
2. Open the project in an editor
3. Run `flutter pub get` to fetch dependencies

### License Configuration
- **Flutter**: Open `example/lib/common/demo_config.dart` and fill in `LicenseUrl` and `LicenseKey`
- **Android**: Modify `applicationId` in `example/android/app/build.gradle.kts`
- **iOS**:
  1. Run `pod install` in the `example/ios` directory
  2. Open `Runner.xcworkspace` with Xcode
  3. Modify the `Bundle Identifier`

### Animation Resource Configuration
1. Add animation files to the `example/asset/anim` directory
2. Modify `_kNormalAnimNames` or `_kTCMP4AnimNames` in `example/lib/tools/demo_asset_helper.dart`
3. Configure the new animation files in `example/pubspec.yaml`

### Run
Select a device and click Run:
- **EffectAnim Demo**: Experience large animation effects
- **EffectAnim List**: Experience small animation (avatar animation) effects
