# Android Platform - Integration Guide & Usage Documentation

## 1. Development Environment Requirements

- **IDE**: Android Studio 2.0+
- **System Version**: Android 4.4 (SDK API 19) and above
- **CPU Architecture**: armeabi-v7a, arm64-v8a

---

## 2. SDK Integration Methods

### Method 1: Gradle Integration (Recommended)

**1. Add dependency** (`app/build.gradle`):
```gradle
implementation "com.tencent.mediacloud:TCEffectPlayer:版本号"
```

**2. Sync Now**

### Method 2: Manual Integration

1. Download `MediaX_Android_SDK_Latest.zip` and extract the AAR files
2. Copy the AAR files to the `app/libs` directory
3. Configure the repository (root `build.gradle`):
```gradle
flatDir {
    dirs 'libs'
}
```
4. Add dependencies (`app/build.gradle`):
```gradle
implementation(name: "TCEffectPlayer_x.x.x", ext: "aar")
implementation(name: "TCMediaX_x.x.x", ext: "aar")
implementation(name: "xmagic-auth_x.x.x", ext: "aar")
```
> Note: Replace x.x.x with the actual version number. The three AARs must share the same version number.

### Configure CPU Architecture

Regardless of the integration method, you need to specify the CPU architecture:
```gradle
defaultConfig {
    ndk {
        abiFilters "armeabi-v7a", "arm64-v8a"
    }
}
```

---

## 3. ProGuard Rules Setting

Add to `proguard-rules.pro`:
```proguard
-keep class com.tcmediax.** { *; }
-keep class com.tencent.** { *; }
-keep class com.tencent.xmagic.** { *; }
# If exifinterface is introduced in the project
-keep class androidx.exifinterface.** {*;}
```

---

## 4. Software Decoding Capability Integration (Recommended)

To address hardware decoding failures on certain devices, it is strongly recommended to integrate the software decoding library (playback success rate can be improved to 99.99%).

- **RT-Cube SDK already integrated (e.g., TRTC/Live)**: Additionally integrate `LiteAVSDK_Player_Mini`
- **RT-Cube Professional SDK already integrated (e.g., Professional/UGC)**: Software decoding is automatically supported
- **RT-Cube SDK not integrated**:
  1. Integrate `LiteAVSDK_Player_Mini`
  2. Download `jniLibs_txffmpeg_so.zip` and copy the so files to `app/src/main/jniLibs`
  3. Configure `sourceSets` in `build.gradle`

---

## 5. License Initialization

You must set the License before using the SDK:

```java
import com.tencent.tcmediax.api.TCMediaXBase;

// Initialize in Application or launch Activity
TCMediaXBase.getInstance().setLicense(context, sLicenseUrl, sLicenseKey,
    new TCMediaXBase.ILicenseCallback() {
        @Override
        public void onResult(int errCode, String msg) {
            if (errCode == 0) {
                // Authorization successful
            } else {
                // Authorization failed, handle error
            }
        }
    });
```

**License Error Code Description**:
| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid input parameter |
| -3 | Download failed, check the network |
| -4, -5 | IO failure, read local authorization info is empty |
| -6 ~ -10 | File format, signature verification, decryption failure, or JSON field error |
| 3015 | Bundle Id / Package Name mismatch |
| 3018 | Authorization file has expired |

**Notes**:
- Make sure the network is available
- For multi-process applications, ensure each process using the player calls this method

---

## 6. Log Management

- **Log Path**: `/sdcard/Android/data/${your_packagename}/files/TCMediaLog`
- **Enable Method**: Disabled by default. Enable via `TCMediaXBase.getInstance().setLogEnable(context, true)`

---

## 7. Player Usage

### 1. Add the player view to layout

```xml
<FrameLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <com.tencent.tcmediax.tceffectplayer.api.TCEffectAnimView
        android:id="@+id/video_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:visibility="visible" />

</FrameLayout>
```

### 2. Initialize the player view

```java
private TCEffectAnimView mPlayerView;
mPlayerView = (TCEffectAnimView) findViewById(R.id.video_view);
// Optional: Set animation alignment
mPlayerView.setScaleType(TCEffectPlayerConstant.ScaleType.FIT_CENTER);
```

### 3. Set the playback listener

```java
playerView.setPlayListener(new TCEffectAnimView.IAnimPlayListener() {
    @Override
    public void onPlayStart() {
        // Animation started playing. getTCAnimInfo() can be called here to get animation info
    }

    @Override
    public void onPlayEnd() {
        // Animation finished playing
    }

    @Override
    public void onPlayError(int errorCode) {
        // Animation playback failed
    }

    @Override
    public void onPlayEvent(int event, Bundle param) {
        // Animation playback event (e.g., progress callback)
    }
});
```

### 4. Playback Control

```java
// Start playback (only supports local resources)
String localPath = "/sdcard/Android/${packageName}/files/tep_cool_ss.mp4";
mPlayerView.startPlay(localPath);

// Pause playback
mPlayerView.pause();

// Resume playback
mPlayerView.resume();

// Loop playback
mPlayerView.setLoop(true);

// Mute playback
mPlayerView.setMute(true);

// Stop playback
mPlayerView.stopPlay(true); // true: clear the last frame
```

> **Note**: Only local resources are supported. Network resources need to be downloaded locally first, and the file suffix must remain unchanged.

---

## 8. Playback Configuration (TCEffectConfig)

Set via `mPlayerView.setConfig(config)`. **Must be called before starting playback**:

```java
TCEffectConfig config = new TCEffectConfig.Builder()
    .setCodecType(TCEffectConfig.CodecType.TC_MPLAYER) // Default decoder
    .setFreezeFrame(TCEffectConfig.FREEZE_FRAME_LAST)  // Stay on the last frame
    .enableProgressCallback(true)                       // Enable progress callback
    .progressCallbackIntervalMs(200)                    // Progress callback interval 200ms
    .build();
mPlayerView.setConfig(config);
```

**Decoder Types**:
- `TC_MPLAYER`: Default decoder
- `TC_MCODEC`: MCODEC decoder
- `TX_LITEAV_SDK`: Tencent Cloud Player SDK (requires additional integration)

---

## 9. Fusion Animation Configuration

### Image Resource Replacement

```java
mPlayerView.setFetchResource(new IFetchResource() {
    @Override
    public void fetchImage(Resource resource, IFetchResourceImgResult result) {
        // Get the corresponding Bitmap based on resource.id
        Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.test);
        result.fetch(bitmap);
    }

    @Override
    public void fetchText(Resource resource, IFetchResourceTxtResult result) {
        // Text replacement
        TCEffectText text = new TCEffectText();
        text.text = "替换文本";
        text.color = 0xFFFFFFFF;
        text.fontSize = 16f;
        text.alignment = TCEffectText.ALIGNMENT_CENTER;
        result.fetch(text);
    }

    @Override
    public void releaseResource(List<Resource> resources) {
        // Release resources
    }
});
```

### Click Event

```java
mPlayerView.setOnResourceClickListener(new OnResourceClickListener() {
    @Override
    public void onClick(Resource resource) {
        // Handle fusion animation element click
    }
});
```

### Update fusion resource during playback (version 3.2+)

```java
mPlayerView.requestUpdateResource();
```

---

## 10. FAQ

### XMAGIC Conflict
If the Beauty Effect SDK is also integrated, conflicts will occur:
- **AAR Integration**: Remove the `xmagic-auth` dependency from the Animation Effect SDK
- **Gradle Integration**: Use `exclude group: "com.tencent.mediacloud", module: "TCXMagicAuth"`

### Encrypted Video Configuration
If using encrypted animations with `targetSDKVersion >= 28`, configure in `res/xml/network_security_config.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">localhost</domain>
        <domain includeSubdomains="true">127.0.0.1</domain>
    </domain-config>
</network-security-config>
```

### TCMP4 Animation Not Displaying
Usually caused by hardware acceleration being disabled. Check whether `android:hardwareAccelerated` in `AndroidManifest` is set to false. TCMP4 requires hardware acceleration to be enabled.

---

## 11. Demo Project

### Environment Preparation
- Android Studio 2.0+, Android SDK API 19+, Android 4.4+

### Steps to Run
1. Download the Android Gift Animation Effect Demo project
2. Open Android Studio, select `Open an Existing Project`, and open `TCEffectPlayerDemo`
3. Replace the AAR files in `app/libs` with the latest version
4. Replace the License info with the valid License you applied for
5. Modify `applicationId` to match the package name bound to the License
6. `Build` -> `Clean Project` to clear the cache
7. Connect a physical device and run

### Animation Resource Configuration
Modify in `MainActivity.java`:
- `FILE_NAME_TEP`: Configure the large animation
- `FILE_NAME_TEPGS`: Configure the small avatar animation

### Verification
The console log `TCMediaX license result: errCode: 0, msg: Success` indicates successful authorization.
