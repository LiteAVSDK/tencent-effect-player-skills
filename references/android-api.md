# Android Platform - API Documentation

## 1. TCMediaXBase

SDK base class, used for initialization and global configuration.

### getInstance
Get the singleton instance of TCMediaXBase.
```java
public static TCMediaXBase getInstance()
```

### setLicense
Set the License authorization.
```java
public void setLicense(Context context, String url, String key, ILicenseCallback callback)
```
| Parameter | Description |
|-----------|-------------|
| `context` | Application Context |
| `url` | License URL |
| `key` | License Key |
| `callback` | Callback interface ILicenseCallback, containing `onResult(int errCode, String msg)` |

**Error Code Description**:
| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid input parameter |
| -3 | Download failed, check the network |
| -4, -5 | IO failure, read local authorization info is empty |
| -6 ~ -10 | File format, signature verification, decryption failure, or JSON field error |
| 3015 | Bundle Id / Package Name mismatch |
| 3018 | Authorization file has expired |

### setLogEnable
Enable or disable Log output. Enabled by default. Logs are saved at `/sdcard/Android/data/packagename/files/TCMediaX`.
```java
public void setLogEnable(Context context, boolean enable)
```

---

## 2. TCEffectAnimView

Effect player view class, responsible for animation playback, control, and event listening.

### startPlay
Start the player. Only supports local sdcard paths.
```java
public int startPlay(String playUrl)
```

### stopPlay
Stop playback.
```java
public void stopPlay(boolean clearLastFrame)
```
| Parameter | Description |
|-----------|-------------|
| `clearLastFrame` | Whether to clear the last frame |

### pause
Pause playback.
```java
public void pause()
```

### resume
Resume playback.
```java
public void resume()
```

### isPlaying
Check whether playback is in progress.
```java
public boolean isPlaying()
```

### setLoop
Set whether to loop playback.
```java
public void setLoop(boolean isLoop)
```

### setLoopCount
Set the number of loop playbacks. Mutually exclusive with `setLoop`; the later call overrides the earlier one.
```java
public void setLoopCount(int loopCount)
```
| Value | Description |
|-------|-------------|
| `<=0` | Infinite loop |
| `>=1` | Number of playbacks |

### setMute
Set mute.
```java
public void setMute(boolean mute)
```

### seekTo
Jump to a specified time point (milliseconds). Only effective for tcmp4 or mp4 decoded with TX_LITEAV_SDK.
```java
public void seekTo(long milliSec)
```

### seekProgress
Jump to playback progress by percentage. Only effective for tcmp4 or mp4 decoded with TX_LITEAV_SDK.
```java
public void seekProgress(float progress)
```
| Parameter | Description |
|-----------|-------------|
| `progress` | 0.0 - 1.0 |

### setDuration
Set the duration required for animation playback to complete (implemented via speed adjustment). Only effective for tcmp4 format; mutually exclusive with `setRate`.
```java
public void setDuration(long durationInMilliSec)
```

### setConfig
Set effect player parameters. **Must be called before starting playback**.
```java
public void setConfig(TCEffectConfig config)
```

### setVideoMode
Set the alpha and rgb region alignment for tep animations.
```java
public void setVideoMode(TCEffectPlayerConstant.VideoMode mode)
```
| Enum Value | Description |
|-----------|-------------|
| `VIDEO_MODE_NONE` | Regular mp4 |
| `VIDEO_MODE_SPLIT_HORIZONTAL` | Horizontal alignment (alpha left / rgb right) |
| `VIDEO_MODE_SPLIT_VERTICAL` | Vertical alignment (alpha top / rgb bottom) |
| `VIDEO_MODE_SPLIT_HORIZONTAL_REVERSE` | Horizontal alignment (rgb left / alpha right) |
| `VIDEO_MODE_SPLIT_VERTICAL_REVERSE` | Vertical alignment (rgb top / alpha bottom) |

### setScaleType
Set the video alignment/scaling mode.
```java
public void setScaleType(TCEffectPlayerConstant.ScaleType type)
```
| Enum Value | Description |
|-----------|-------------|
| `FIT_XY` | Fully fill the entire layout (default) |
| `FIT_CENTER` | Fully display centered with aspect ratio |
| `CENTER_CROP` | Fully fill layout proportionally, cropping excess parts |

### setRenderRotation
Set the render rotation angle (supports 0, 90, 180, 270, 360 degrees).
```java
public void setRenderRotation(int rotation)
```

### setFetchResource
Set the resource fetcher interface for fusion animations.
```java
public void setFetchResource(IFetchResource fetchResource)
```
The IFetchResource interface contains:
- `fetchImage(Resource resource, IFetchResourceImgResult result)` - Fetch image resource
- `fetchText(Resource resource, IFetchResourceTxtResult result)` - Fetch text resource
- `releaseResource(List<Resource> resources)` - Release resources

### setOnResourceClickListener
Set the click event for fusion animation resources.
```java
public void setOnResourceClickListener(OnResourceClickListener listener)
```
OnResourceClickListener interface: `onClick(Resource resource)`

### requestUpdateResource
Update fusion animation info during playback (supported in version 3.2+).
```java
public void requestUpdateResource()
```

### setAudioFrameDataListener
Listen to audio track data during animation playback (supported in version 3.3+).
```java
public void setAudioFrameDataListener(ITCEffectAudioFrameDataListener audioFrameDataListener)
```
> It is recommended to copy the callback data when used in business logic.

### preloadTCAnimInfo (static method)
Preload and get animation info (width/height, duration, fusion attributes, etc.) (supported in version 3.3+).
```java
public static TCEffectAnimInfo preloadTCAnimInfo(String playUrl, TCEffectConfig config)
```
> Involves IO operations. It is recommended not to call it on the main thread. Old vap animations (without the vapc box) cannot be parsed and will return null.

### setPlayListener
Set the playback status callback.
```java
public void setPlayListener(IAnimPlayListener listener)
```
IAnimPlayListener callback methods:
- `onPlayStart()` - Playback started
- `onPlayEnd()` - Playback ended
- `onPlayError(int errorCode)` - Playback error
- `onPlayEvent(int event, Bundle param)` - Playback event (first frame display, resolution change, progress update, etc.)

### getTCAnimInfo
Get detailed information about the currently playing animation. **Must be called after onPlayStart**.
```java
public TCEffectAnimInfo getTCAnimInfo()
```

### getTCEffectPlayer
Get the TCEffectPlayer instance.
```java
public TCEffectPlayer getTCEffectPlayer()
```

### onDestroy
Stop playback and release resources.
```java
public void onDestroy()
```

---

## 3. TCEffectConfig

Effect player configuration class, built via the Builder pattern.

### Builder Methods

| Method | Description | Values |
|--------|-------------|--------|
| `setCodecType(CodecType type)` | Set the decoder type (only effective for TEP animations) | `TC_MPLAYER` (default), `TC_MCODEC`, `TX_LITEAV_SDK` |
| `setFreezeFrame(int frame)` | Set the frozen retention frame | `FREEZE_FRAME_NONE` (off), `FREEZE_FRAME_LAST` (last frame) |
| `setAnimType(AnimType type)` | Specify the animation format (ignores file suffix) | `AUTO` (default), `MP4`, `TCMP4` |
| `enableAudioFrameCallback(boolean)` | Enable audio frame callback | Only effective with TX_LITEAV_SDK + MP4/TEP |
| `enableProgressCallback(boolean)` | Enable progress callback | - |
| `progressCallbackIntervalMs(int)` | Progress callback interval (milliseconds) | Default 200ms |

---

## 4. TCEffectAnimInfo

Stores information about the currently playing animation.

| Property | Type | Description |
|----------|------|-------------|
| `type` | int | Animation type (MP4 or TCMP4) |
| `duration` | long | Animation duration (milliseconds) |
| `width` | int | Width |
| `height` | int | Height |
| `encryptLevel` | int | Encryption type |
| `mixInfo` | MixInfo | Fusion animation info |

### MixInfo
| Property | Type | Description |
|----------|------|-------------|
| `textMixItemList` | List\<MixItem\> | Text fusion list |
| `imageMixItemList` | List\<MixItem\> | Image fusion list |

### MixItem
| Property | Type | Description |
|----------|------|-------------|
| `id` | String | Fusion animation ID |
| `tag` | String | Fusion animation Tag |
| `text` | String | Text content (empty for image fusion) |

---

## 5. TCEffectText

Text style class for fusion animation replacement.

| Property | Type | Description |
|----------|------|-------------|
| `text` | String | Final displayed text |
| `color` | int | Text color (ARGB, e.g., 0xFFFFFFFF) |
| `fontStyle` | String | Style ("bold" for bold) |
| `alignment` | int | Alignment (NONE/LEFT/CENTER/RIGHT) |
| `fontSize` | float | Font size |

---

## 6. TCEffectAudioFrameData

Audio track callback data class (version 3.3+).

| Property | Type | Description |
|----------|------|-------------|
| `data` | byte[][] | Audio sample data |
| `size` | int[] | Data size |
| `format` | int | Audio format |
| `channelLayout` | long | Channel layout |
| `sampleRate` | int | Sample rate |
| `channels` | int | Number of channels |
| `nbSamples` | int | Samples per channel |
| `ptsMs` | long | Current PTS |

---

## 7. Common Error Codes

| Error Code | Description |
|-----------|-------------|
| -10002 | Hardware decoding not supported (low-end device) |
| -10003 | Thread creation failed |
| -10004 | Render creation failed |
| -10006 | File cannot be read |
| -10007 | The H265 animation video encoding format is not supported on the current device |
| -10009 | Invalid License |
| -10010 | MediaPlayer playback failed |
| -10011 | Hardware decoding not supported |
| -10012 | Missing required dependency (e.g., TX_LITEAV_SDK not imported when used) |
