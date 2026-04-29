# FAQ

## 1. Playback Failure Error Code Troubleshooting

### -10009: License Validation Failed
**Cause**: License info mismatch, expired, or network authentication failure.

**Troubleshooting Steps**:
1. Check whether `setLicense` was called with correct URL/KEY
2. Confirm packageName/bundleId matches the one used during application
3. Check whether the License has expired
4. Check network connection (on iOS, set after network permission is granted)
5. Check the specific error code in the callback (3015 = package name mismatch, 3018 = authorization expired)

### -10002 / -10011: Hardware Decoding Not Supported
**Cause**: Low-end devices lack sufficient hardware decoding capability.

**Solutions**:
1. Keep MP4 resolution under 1080p (recommended to use the conversion tool to generate compatible resolutions)
2. Integrate the software decoding capability (auto-switches to software decoding when hardware decoding fails)

### -10007: H265 Not Supported
The current device does not support H.265 encoding.

### -10012: Missing Required Dependency
Using the TX_LITEAV_SDK decoder but the related SDK is not imported.

---

## 2. Log Management

### How to Extract Runtime Logs
- **Android**: `/sdcard/Android/data/${your_packagename}/files/TCMediaLog`
- **iOS**: Sandbox `Documents/TCMedialog` (export via Xcode > Window > Devices and Simulators)

### No Log Files
Check whether `setLogEnable(true)` was called to enable the logging feature (disabled by default).

---

## 3. Format and Compression Ratio

### Support for Legacy Transparent MP4
Compatible with old transparent VAP videos. Additionally provides an Effect Conversion Tool supporting conversion from MP4, SVGA, PAG, Lottie, and other formats.

### TCMP4 Compression Advantage
Compared with formats such as SVGA, TCMP4 reduces file size by approximately **81.5%** (pure binary data structure, dynamic bit storage, and centralized compression of similar blocks).

---

## 4. Technical Details

### Decoding Method
Primarily uses **hardware decoding** to reduce CPU usage and improve stability.

### Relationship with RT-Cube
Since version 3.0, SDK features have been integrated into the Effect Player, and it no longer depends on the Tencent Cloud VOD Player.

### Recommended Resolution
Recommended not to exceed **1080p** (1920px x 1080px).

---

## 5. Android Platform Specific Issues

### TCMP4 Animation Not Displaying (Consistent)
Usually caused by hardware acceleration being disabled. Check:
- Whether `android:hardwareAccelerated` in `AndroidManifest` is set to false
- Whether `view.setLayerType` is called in the code to disable hardware acceleration
- **TCMP4 requires hardware acceleration to be enabled**

### x86 Architecture Support
Supported starting from version **3.3.0.251** for x86 64-bit architecture.

---

## 6. Usage Scenario Recommendations

### MP4 Format
- Suitable for **large animation** scenarios (e.g., full-screen gifts in live rooms)
- Recommended to use one instance per page, reused via a queue
- Not recommended to play multiple MP4s simultaneously

### TCMP4 Format
- Suitable for **small animation** scenarios (e.g., avatar frames, like animations)
- Supports multiple instances playing simultaneously (64 avatar frames on screen)

---

## 7. Common Error Codes Summary

| Error Code | Description |
|-----------|-------------|
| -10002 | Hardware decoding not supported (low-end device) |
| -10003 | Thread creation failed |
| -10004 | Render creation failed |
| -10006 | File cannot be read |
| -10007 | H265 encoding not supported on the current device |
| -10009 | Invalid License |
| -10010 | MediaPlayer playback failed |
| -10011 | Hardware decoding not supported |
| -10012 | Missing required dependency |
