# iOS Platform - Integration Guide & Usage Documentation

## 1. Development Environment Requirements

- **Xcode**: 9.0 or higher
- **Device**: iPhone or iPad physical device running iOS 9.0 or above
- **Signing**: Project configured with valid developer signing

---

## 2. SDK Integration Methods

### Method 1: CocoaPods Integration (Recommended)

Add to `Podfile`:
```ruby
# Import TCMediaX
pod 'TCMediaX', :podspec => 'https://mediacloud-76607.gzc.vod.tencent-cloud.com/MediaX/iOS/podspec/release/3.4/3.4.244/TCMediaX.podspec'

# Import TCEffectPlayer
pod 'TCEffectPlayer', :podspec => 'https://mediacloud-76607.gzc.vod.tencent-cloud.com/MediaX/iOS/podspec/release/3.4/3.4.244/TCEffectPlayer.podspec'

# Import YTCommonXMagic.framework
pod 'YTCommonXMagic', :podspec => 'https://mediacloud-76607.gzc.vod.tencent-cloud.com/MediaX/iOS/podspec/release/YTCommonXMagic_1.3.1/YTCommonXMagic.podspec'
```

> **Decoder Configuration Notes**:
> - **Standalone Integration** (does not depend on Tencent Cloud Player SDK): Set `TCEffectConfig#vapEngineType` to `TCEPCodecTypeAVPlayer`
> - **Linked Integration** (depends on Tencent Cloud Player SDK): Set to `TCEPCodecTypeVODPlayer` (using the `LiteAVSDK_Player_Mini` streamlined version does not require an additional Player License)

### Method 2: Manual Integration

1. Download `MediaX_Android_iOS_Latest.zip` and extract
2. Integrate the following frameworks:

**SDK Libraries**:
- `TCMediaX.xcframework`
- `TCEffectPlayer.xcframework`
- `libtcpag.xcframework`
- `YTCommonXMagic.framework`

**System Libraries**:
- `libz.tbd`
- `libc++.tbd`
- `AVFoundation.framework`

3. Add `-ObjC` in "Build Settings" > "Other Linker Flags"

---

## 3. License Initialization

```objective-c
#import <TCMediaX/TCMediaX.h>

@interface AppDelegate () <TCMediaXBaseDelegate>
@end

@implementation AppDelegate

static NSString *LICENCE_URL = @"Replace with your License URL";
static NSString *LICENCE_KEY = @"Replace with your License Key";

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [[TCMediaXBase getInstance] setLicenceURL:LICENCE_URL key:LICENCE_KEY];
    [[TCMediaXBase getInstance] setDelegate:self];
    return YES;
}

#pragma mark - TCMediaXBaseDelegate
- (void)onLicenseCheckCallback:(int)errcode withParam:(NSDictionary *)param {
    NSLog(@"onLicenseCheckCallback:%d", errcode);
    if (errcode == TMXLicenseCheckOk) {
        // Authorization successful
    } else {
        // Authorization failed, retry logic is required
    }
}
@end
```

> **Note**: License verification is strictly online. If there is no network permission on first launch, call again after authorization.

---

## 4. Log Management

- **Log Path**: Sandbox `Documents/TCMedialog` folder
- **Enable Method**: Call `[[TCMediaXBase getInstance] setLogEnable:YES]`
- **Export Method**: Export via Xcode > Window > Devices and Simulators

---

## 5. Player Usage

### 1. Initialization and Layout

```objective-c
- (TCEffectAnimView *)alphaAnimView {
    if (!_alphaAnimView) {
        _alphaAnimView = [[TCEffectAnimView alloc] init];
        _alphaAnimView.backgroundColor = [UIColor clearColor];
        _alphaAnimView.effectPlayerDelegate = self;
        _alphaAnimView.loop = YES;
        [_alphaAnimView setRenderMode:TCEPVPViewContentModeScaleToFill];
    }
    return _alphaAnimView;
}

// Add view and layout
[self.view addSubview:self.alphaAnimView];
self.alphaAnimView.frame = self.view.bounds;
```

### 2. Playback Listening

```objective-c
#pragma mark - TCEPAnimViewDelegate

- (void)tcePlayerStart:(ITCEffectPlayer *)player {
    NSLog(@"player = %p, start..", player);
}

- (void)tcePlayerEnd:(ITCEffectPlayer *)player {
    NSLog(@"player = %p, end..", player);
}

- (void)tcePlayerError:(ITCEffectPlayer *)player error:(NSError *)error {
    NSLog(@"player = %p, error..", player);
}

- (void)onPlayEvent:(ITCEffectPlayer *)player event:(int)EvtID withParam:(NSDictionary *)param {
    NSLog(@"player = %p, EvtID = %@", player, @(EvtID));
}
```

### 3. Playback Resources

Only local resources are supported (`.mp4`, `.tcmp4`, VAP formats):
```objective-c
NSString *mp4Url = @"xxx/xxx/tuiad_vapx_cool_sss.mp4";
[self.alphaAnimView startPlay:mp4Url];
```

### 4. Playback Control

```objective-c
// Pause playback
[self.alphaAnimView pause];

// Resume playback
[self.alphaAnimView resume];

// Stop playback (release resources)
[self.alphaAnimView stopPlay];

// Mute
[self.alphaAnimView setMute:YES];

// Loop playback
[self.alphaAnimView setLoop:YES];
```

---

## 6. Playback Configuration

```objective-c
TCEffectConfig *config = [[TCEffectConfig alloc] init];
// Set playback engine
config.vapEngineType = TCEPCodecTypeAVPlayer; // Default engine
// Set freeze frame (stay on the last frame)
config.freezeFrame = FRAME_LAST;
// Enable progress callback
config.enableProgressCallback = YES;
config.progressCallbackIntervalMs = 200;
[self.alphaAnimView setEffectPlayerConfig:config];
```

---

## 7. Fusion Animation Configuration

### Text Replacement

```objective-c
- (TCEffectText *)loadTextForPlayer:(ITCEffectPlayer *)player withTag:(NSString *)tag {
    if (tag != nil && [tag isEqualToString:@"name"]) {
        TCEffectText *effectText = [[TCEffectText alloc] init];
        effectText.text = @"reText";
        effectText.alignment = TCEPTextAlignmentLeft;
        return effectText;
    }
    return nil;
}
```

### Image Replacement

```objective-c
- (void)loadImageForPlayer:(ITCEffectPlayer *)player
                   context:(NSDictionary *)context
                completion:(void(^)(UIImage *image, NSError *error))completionBlock {
    dispatch_async(dispatch_get_main_queue(), ^{
        NSString *tag = context[TCEPContextSourceTypeImageIndex];
        UIImage *image = [self getImageForTag:tag];
        if (image) {
            completionBlock(image, nil);
        } else {
            completionBlock(nil, error);
        }
    });
}
```

### Click Event

Listen to fusion animation resource click events via the `tcePlayerTagTouchBegan:tag:` callback of `TCEPAnimViewDelegate`.

---

## 8. FAQ

### YTCommonXMagic Conflict
If the Beauty Effect SDK is already integrated, both share `YTCommonXMagic.framework`, so no need to import it again.

### Success Verification
The console log `TCMediaXBase setLicense authresult: 0 ,errorMsg: { }` indicates successful authorization.

---

## 9. Demo Project

### Steps to Run
1. Download the iOS Demo project
2. Enter the `TCEffectPlayerDemo_iOS` directory and run `pod install`
3. Open `TCEffectPlayerDemo.xcworkspace`
4. Replace `LICENCE_URL` and `LICENCE_KEY` in `AppDelegate.m`
5. Modify the Target's `Bundle Id` to match the Bundle Id bound to the License
6. Import the certificate and connect a physical device to run

### Animation Resource Configuration
Place animation resources in the `Resource` directory and configure the animation resource names to play in `ViewController.m`.

> **Note**: First-time authorization requires an internet connection. Make sure the device has a stable network.
