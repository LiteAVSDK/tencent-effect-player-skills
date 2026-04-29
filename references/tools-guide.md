# Effect Conversion Tool and Preview Tool

## 1. Effect Conversion Tool (TEP Tools)

A material generation and conversion tool that converts various formats into the `.mp4` or `.tcmp4` formats supported by the Effect Player.

### System Requirements
- macOS 14 and above (supports x86 and arm64)

### Download and Installation
1. Download `TEP_Tool_Latest.zip` and extract
2. Open the dmg file and drag `TEP Tools.app` to the `Applications` folder
3. **Do not** run directly from within the dmg environment
4. If a prompt appears on first launch, go to `System Preferences > Privacy & Security` and click "Open Anyway"

### Basic Configuration
- **License Info**: Fill in `appId` and `LicenseKey` (multiple package names separated by commas)
- **fps**: Frame rate
- **Alpha Scale**: Alpha region scaling (default 0.5); reducing resolution improves compatibility
- **Anim Encrypt**: Encryption switch (after encryption, network security policy must be configured)
- **Compatibility mode**: Compatibility mode, controls output to not exceed 1080p

### Supported Format Conversions

| Source Format | Description |
|--------------|-------------|
| PNG Frame Sequence | Frame images incrementing from `000.png`; supports adding mp3 audio |
| SVGA | Single or batch conversion; supports dynamic item |
| WebP | Animated WebP; batch supported |
| PAG | PAG animation files; batch supported |
| Lottie | Lottie animations; batch supported |

### Fusion Animation Conversion (PNG Frame Sequence)
- Mask frame size must be consistent with the video frame; naming rules are the same
- Black areas indicate display positions; other areas have transparency 0
- Supports configuring source type (image/text), fit type, text color, font size

### FAQ
- **"Video resolution exceeds 1504"**: Check Compatibility mode so that the output does not exceed 1080p

---

## 2. Effect Preview Tool (TEP Preview)

A local preview tool for quickly validating animation material effects.

### System Requirements
- macOS only

### Download and Installation
Download `TEP_Preview_Latest.zip`, extract it, and double-click `TEP Preview` to open.

### Usage Workflow

#### Preview Animation
1. Fill in the License Key
2. Drag an animation file into the preview area
3. Use the buttons in the lower-left to control pause/play

#### View Detailed Info
Click the lower-right area to view:
- **Fusion Animation Info**: Replaceable image/text elements and their types
- **Animation Parameters**: Duration, FrameRate, Width/Height (resolution), FrameCount (total frames)

### Recommendation
After conversion, use the preview tool to verify the animation effects. Only distribute to the mobile SDK after confirming correctness.
