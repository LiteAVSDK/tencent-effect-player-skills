# Flutter Platform - API Documentation

## 1. FTCMediaXBase

Global management class, responsible for singleton and License authorization.

### instance
Get the singleton of FTCMediaXBase.
```dart
FTCMediaXBase.instance
```

### setLicense
Set the License authorization.
```dart
void setLicense(String url, String key, Function(int errCode, String msg) callback)
```
| Parameter | Description |
|-----------|-------------|
| `url` | License URL |
| `key` | License Key |
| `callback` | Callback function; errCode 0 indicates success |

**Error Code Description**:
| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid parameter |
| -3 | Download failed (network issue) |
| 3015 | Bundle Id / Package Name mismatch |
| 3018 | Authorization file expired |

### setLogEnable
Set whether Log output is enabled. Enabled by default.
```dart
void setLogEnable(bool enable)
```

---

## 2. FTCEffectAnimView

UI component used to create the gift animation playback view.

### Constructor
```dart
FTCEffectAnimView({
  required Function(FTCEffectViewController controller) controllerCallback,
})
```
| Parameter | Description |
|-----------|-------------|
| `controllerCallback` | Callback function used to obtain the playback controller `FTCEffectViewController` |

---

## 3. FTCEffectViewController

Core playback controller, providing all operation methods for animation playback.

### Playback Control

#### startPlay
Start playback. **Only supports local video resources**.
```dart
Future<void> startPlay(String playUrl)
```

#### stopPlay
Stop playback.
```dart
Future<void> stopPlay()
```

#### pause
Pause playback.
```dart
Future<void> pause()
```

#### resume
Resume playback.
```dart
Future<void> resume()
```

#### isPlaying
Query whether playback is in progress.
```dart
Future<bool> isPlaying()
```

#### seekTo
Jump to a specified time (milliseconds). Only effective for tcmp4 or specific engines.
```dart
Future<void> seekTo(int milliSec)
```

#### seekProgress
Jump to a specified percentage (0.0 - 1.0). Only effective for tcmp4 or specific engines.
```dart
Future<void> seekProgress(double progress)
```

### Configuration

#### setConfig
Set player parameters. **Must be called before playback**.
```dart
Future<void> setConfig(FTCEffectConfig config)
```

#### setVideoMode
Set the alpha and rgb region alignment for tep animations.
```dart
Future<void> setVideoMode(FTCEffectVideoMode mode)
```
| Enum Value | Description |
|-----------|-------------|
| `none` | Regular mp4 |
| `splitHorizontal` | Horizontal alignment (alpha left / rgb right) |
| `splitVertical` | Vertical alignment (alpha top / rgb bottom) |
| `splitHorizontalReverse` | Horizontal alignment (rgb left / alpha right) |
| `splitVerticalReverse` | Vertical alignment (rgb top / alpha bottom) |

#### setScaleType
Set the view alignment mode.
```dart
Future<void> setScaleType(FTCEffectScaleType type)
```
| Enum Value | Description |
|-----------|-------------|
| `fitXY` | Full fill (default) |
| `fitCenter` | Centered with aspect ratio |
| `centerCrop` | Fill proportionally and crop |

#### setRenderRotation
Set the rotation angle.
```dart
Future<void> setRenderRotation(int rotation)
```

#### setLoop
Set whether to loop playback.
```dart
Future<void> setLoop(bool loop)
```

#### setLoopCount
Set the number of loop playbacks.
```dart
Future<void> setLoopCount(int count)
```
| Value | Description |
|-------|-------------|
| `<=0` | Infinite loop |
| `>=1` | Play n times |

#### setDuration
Set the total animation duration (will adjust playback speed). Only effective for tcmp4.
```dart
Future<void> setDuration(int durationInMilliSec)
```

#### setMute
Set mute.
```dart
Future<void> setMute(bool mute)
```

### Resources and Fusion

#### setFetchResource
Set the resource fetcher for fusion animations (images, text).
```dart
void setFetchResource({
  Future<Uint8List?> Function(FResource resource)? imgFetcher,
  Future<FTCEffectText?> Function(FResource resource)? textFetcher,
})
```

#### requestUpdateResource
Request to update fusion resources during playback.
```dart
Future<void> requestUpdateResource()
```

### Status and Info

#### getTCAnimInfo
Get detailed info of the current animation. **Must be called after the onPlayStart callback**.
```dart
Future<FTCEffectAnimInfo?> getTCAnimInfo()
```

#### setPlayListener
Set the playback status listener.
```dart
void setPlayListener({
  VoidCallback? onPlayStart,
  VoidCallback? onPlayEnd,
  Function(int event, Map param)? onPlayEvent,
  Function(int errorCode)? onPlayError,
})
```

---

## 4. FTCEffectConfig

Player configuration class.

| Property | Type | Description |
|----------|------|-------------|
| `codecType` | FTCEffectCodecType | Codec type |
| `freezeFrame` | FTCEffectFreezeFrame | Freeze frame setting |
| `animType` | FTCEffectAnimType | Specify animation format (Android only) |
| `enableProgressCallback` | bool | Whether to enable progress callback |
| `progressCallbackIntervalMs` | int | Progress callback interval (milliseconds), default 200ms |

**codecType enum**:
| Value | Description |
|-------|-------------|
| `TC_MPLAYER` | Default MPLAYER engine |
| `TC_MCODEC` | MCODEC engine (Android only) |
| `TX_LITEAV_SDK` | Tencent Cloud Player SDK (requires additional import and authorization) |

**freezeFrame enum**:
| Value | Description |
|-------|-------------|
| `NONE` | Disabled |
| `LAST` | Stay on the last frame |

**animType enum** (Android only):
| Value | Description |
|-------|-------------|
| `AUTO` | Auto recognize (default) |
| `MP4` | MP4 format |
| `TCMP4` | TCMP4 format |

---

## 5. FTCEffectAnimInfo

Stores info of the current animation.

| Property | Type | Description |
|----------|------|-------------|
| `type` | int | Animation type (MP4 or TCMP4) |
| `duration` | int | Duration (milliseconds) |
| `width` | int | Width |
| `height` | int | Height |
| `encryptLevel` | int | Encryption level |
| `mixInfo` | FMixInfo? | Fusion info |

---

## 6. FMixInfo

Stores fusion info.

| Property | Type | Description |
|----------|------|-------------|
| `textMixItemList` | List\<FMixItem\> | Text fusion list |
| `imageMixItemList` | List\<FMixItem\> | Image fusion list |

---

## 7. FMixItem

Single fusion item info.

| Property | Type | Description |
|----------|------|-------------|
| `id` | String | Fusion animation ID |
| `tag` | String | Fusion animation Tag |
| `text` | String | Text content (empty for image fusion) |

---

## 8. FTCEffectText

Fusion text style data.

| Property | Type | Description |
|----------|------|-------------|
| `text` | String | Final displayed text |
| `color` | int | Text color (ARGB) |
| `fontSize` | double | Font size |
| `alignment` | int | Alignment |
| `fontStyle` | String | Style |
