# iOS Platform - API Documentation

## 1. TCMediaXBase

SDK base management class, responsible for initialization, authorization, and log management.

### getInstance
Get the singleton of TCMediaXBase.
```objective-c
+ (instancetype)getInstance;
```

### setLicenceURL:key:
Set the License.
```objective-c
- (void)setLicenceURL:(NSString *)url key:(NSString *)key;
```

### setDelegate:
Set the License registration result callback.
```objective-c
- (void)setDelegate:(id<TCMediaXBaseDelegate>)delegate;
```

### setLogEnable:
Set whether Log output is enabled. Enabled by default.
```objective-c
- (void)setLogEnable:(BOOL)enabled;
```

### getSdkVersion
Get the current SDK version.
```objective-c
- (NSString *)getSdkVersion;
```

**License Validation Error Codes (onLicenseCheckCallback callback)**:
| Error Code | Description |
|-----------|-------------|
| 0 | Success |
| -1 | Invalid input parameter |
| -3 | Download failed, please check the network |
| -4 | Reading local authorization info is empty (IO failure) |
| -13 | Authentication failed |

---

## 2. TCEffectAnimView

Core playback view class, responsible for animation lifecycle management and rendering.

### Initialization and Playback

#### initWithFrame:
Create a playback view object.
```objective-c
- (instancetype)initWithFrame:(CGRect)frame;
```

#### startPlay:
Start the player. **Only supports local video resources**.
```objective-c
- (void)startPlay:(NSString *)url;
```

#### stopPlay
Stop playback.
```objective-c
- (void)stopPlay;
```

#### setEffectPlayerConfig:
Set effect player parameters. **Must be called before starting playback**.
```objective-c
- (void)setEffectPlayerConfig:(TCEffectConfig *)config;
```

### Rendering and Mode Settings

#### setVideoMode:
Set the alpha and RGB region alignment for MP4 animations.
```objective-c
- (void)setVideoMode:(TCEPVAPVFTextureBlendMode)mode;
```
| Enum Value | Description |
|-----------|-------------|
| `TCEPVAPVFTextureBlendMode_None` | Regular MP4 |
| `AlphaLeft` | Horizontal alignment (alpha left) |
| `AlphaRight` | Horizontal alignment (alpha right) |
| `AlphaTop` | Vertical alignment (alpha top) |
| `AlphaBottom` | Vertical alignment (alpha bottom) |

#### setRenderMode:
Set the view content fill mode.
```objective-c
- (void)setRenderMode:(TCEPVPViewContentMode)mode;
```
| Enum Value | Description |
|-----------|-------------|
| `ScaleToFill` | Stretch to fill (may distort) |
| `AspectFit` | Maintain aspect ratio, fully display |
| `AspectFill` | Maintain aspect ratio, fill the view (may crop) |

#### setRenderRotation:
Set the animation rotation direction.
```objective-c
- (void)setRenderRotation:(TCEPHomeOrientation)rotation;
```
| Enum Value | Description |
|-----------|-------------|
| `TCEP_HOME_ORIENTATION_DOWN` | Portrait (common) |
| `TCEP_HOME_ORIENTATION_RIGHT` | Landscape (Home button on right) |
| `TCEP_HOME_ORIENTATION_LEFT` | Landscape (Home button on left) |

### Playback Progress Control

#### pause
Pause playback.
```objective-c
- (void)pause;
```

#### resume
Resume playback.
```objective-c
- (void)resume;
```

#### isPlaying
Whether playback is in progress.
```objective-c
- (BOOL)isPlaying;
```

#### seek:
Jump to a specified time (milliseconds). Only effective for tcmp4 or specific MP4 playback engines.
```objective-c
- (void)seek:(long)milliSec;
```

#### seekProgress:
Jump to a specified percentage (0.0 - 1.0). Only effective for tcmp4 or specific MP4 playback engines.
```objective-c
- (void)seekProgress:(float)progress;
```

#### setLoop:
Set whether to loop playback.
```objective-c
- (void)setLoop:(BOOL)loop;
```

#### setLoopCount:
Set the number of loop playbacks.
```objective-c
- (void)setLoopCount:(int)loopCount;
```
| Value | Description |
|-------|-------------|
| `<=0` | Infinite loop |
| `>=1` | Play n times (default 1) |

#### setDuration:
Force the animation playback duration (implemented via speed adjustment). Only effective for tcmp4.
```objective-c
- (void)setDuration:(long)durationInMilliSec;
```

### Other Features

#### setMute:
Set mute.
```objective-c
- (void)setMute:(BOOL)mute;
```

#### getAnimInfo
Get info about the currently playing animation (width/height, duration, etc.). **Must be called after playback has started**.
```objective-c
- (TCEffectAnimInfo *)getAnimInfo;
```

#### requestUpdateResource
Request to update fusion animation info (version 3.2+).
```objective-c
- (void)requestUpdateResource;
```

#### preloadTCAnimInfo:config:
Preload animation info (involves IO operations, be mindful of threads).
```objective-c
+ (TCEffectAnimInfo *)preloadTCAnimInfo:(NSString *)playUrl config:(TCEffectConfig *)config;
```

---

## 3. TCEPAnimViewDelegate Protocol

Listen to playback events by setting `effectPlayerDelegate` or implementing this protocol.

### Playback Status Callbacks

| Callback Method | Description |
|----------------|-------------|
| `tcePlayerStart:` | Animation started playing |
| `tcePlayerEnd:` | Animation finished playing |
| `tcePlayerError:error:` | Animation playback error |
| `onPlayEvent:event:withParam:` | Player event notification (e.g., progress update) |
| `tcePlayerTagTouchBegan:tag:` | Fusion animation resource click event |

### Data Replacement Callbacks (Fusion Animation)

| Callback Method | Description |
|----------------|-------------|
| `loadTextForPlayer:withTag:` | Replace text placeholder, returns a `TCEffectText` object |
| `loadImageForPlayer:context:completion:` | Replace image resource |

### Audio Data Callback

| Callback Method | Description |
|----------------|-------------|
| `tcePlayerAudioFrame:` | Audio frame data callback during animation playback, returns `TCEffectAudioFrameData` |

---

## 4. TCEffectConfig

Playback configuration class.

| Property | Type | Description |
|----------|------|-------------|
| `vapEngineType` | TCEPCodecType | Playback engine type |
| `freezeFrame` | int | Freeze frame setting |
| `enableAudioFrameCallback` | BOOL | Whether to enable audio frame callback (requires VODPlayer engine) |
| `enableProgressCallback` | BOOL | Whether to enable progress callback |
| `progressCallbackIntervalMs` | int | Progress callback interval (milliseconds), default 200ms |

**vapEngineType enum**:
| Value | Description |
|-------|-------------|
| `TCEPCodecTypeAVPlayer` | Default engine |
| `TCEPCodecTypeVODPlayer` | Tencent Cloud Player SDK engine (requires separate import and authorization) |

**freezeFrame enum**:
| Value | Description |
|-------|-------------|
| `FRAME_NONE` | Disabled |
| `FRAME_FIRST` | Stay on the first frame |
| `FRAME_LAST` | Stay on the last frame |

---

## 5. TCEffectAnimInfo

Stores metadata of the playing animation.

| Property | Type | Description |
|----------|------|-------------|
| `type` | int | Animation type (MP4 or TCMP4) |
| `duration` | long | Duration (milliseconds) |
| `width` | int | Width |
| `height` | int | Height |
| `encryptLevel` | int | Encryption type |

---

## 6. TCEffectText

Used to replace text in animations.

| Property | Type | Description |
|----------|------|-------------|
| `text` | NSString | Text content |
| `color` | int | Color (ARGB) |
| `fontSize` | float | Font size |
| `alignment` | int | Alignment |
| `fontStyle` | NSString | Style |

---

## 7. TCEffectAudioFrameData

Contains detailed information about audio PCM data.

| Property | Type | Description |
|----------|------|-------------|
| `data` | pointer | Audio sample data pointer |
| `size` | int | Data size |
| `sampleRate` | int | Sample rate |
| `channels` | int | Number of channels |
| `ptsMs` | long | Timestamp |
