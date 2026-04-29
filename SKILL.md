---
name: tencent-gift-effect-skills
description: "This skill provides comprehensive guidance for integrating and using the Tencent Gift Animation Effect SDK (腾讯礼物动画特效 SDK) across Android, iOS, and Flutter platforms. It should be used when the user needs to implement gift animation playback functionality using TCEffectAnimView, configure effect player settings, handle playback events, manage fusion animations, or set up License authorization. Trigger keywords include: 腾讯礼物动画特效SDK, Gift AR, 礼物播放器, 特效播放器, TCEffectAnimView, TCEffectPlayer, TCMediaXBase, 礼物动画, 动画特效, 融合动画, TEP, TCMP4, 特效转换工具, 特效预览工具, FTCEffectAnimView, TCEffectConfig, FTCMediaXBase, 礼物特效License."
license: Apache-2.0
metadata: 
    author: kakaayang
    version: 1.0
---

# Tencent Gift Animation Effect SDK

## Product Overview

Tencent Gift Animation Effect (part of Tencent RT-Cube) is a core component of the Tencent Effect SDK, providing a high-performance, highly compatible, and highly secure animation effect playback solution, suitable for interactive gifting scenarios.

### Core Advantages
- **Multi-format Effect Support**: Supports playback after conversion from MP4, VAP, SVGA, PAG, WebP, Lottie, PNG sequence frames, and other formats
- **High-performance Playback**: Lower performance overhead when playing complex effects, supports multiple animations playing simultaneously (40+ avatar frames + 1 large animation)
- **Strong Device Compatibility**: Deeply optimized for smooth experience on mid-to-low-end devices
- **High Security**: Effect resources are encrypted to prevent piracy and tampering

### Supported Platforms
- **Android**: API 19+ (Android 4.4+), supports armeabi-v7a, arm64-v8a
- **iOS**: iOS 9.0+, iPhone/iPad physical devices
- **Flutter**: Flutter 3.16.0+

### Core Features

#### Effect Format Conversion
Supports converting PNG frame sequences, SVGA, WebP animations, PAG animations, and Lottie into `.mp4` or `.tcmp4` format.

#### Effect Capabilities
- **Regular Gift Effects**: Particle effects, path animations, scale/rotation animations
- **Fusion Animation Effects**: Dynamically replace assets (text/images) during playback, customize font color, size, and alignment
- **Animation Interaction**: Custom resource click event callbacks
- **MP4 Animation Playback**: Supports MP4 animations with alpha channel (VAP), compatible with various alpha channel layouts
- **Simultaneous Multi-animation Playback**: 40+ avatar frame animations + 1 large entrance animation playing smoothly at the same time
- **Software/Hardware Decoding**: Hardware decoding priority, automatic fallback to software decoding on failure, 99.99% playback success rate
- **Last Frame Retention**: Can stay on the last frame after playback ends

#### Playback Control
- Basic controls: start, stop, pause, resume
- Loop playback, seek, background playback
- Playback callbacks: status notifications (start, complete, failure)

### Animation Format Recommendations
- **MP4 Format**: Suitable for large animation scenarios (e.g., full-screen gifts in live rooms). It is recommended to use one instance per page and reuse it via a queue
- **TCMP4 Format**: Suitable for small animation scenarios (e.g., avatar frames, likes), supports multiple instances playing simultaneously (64 on screen)
- TCMP4 reduces file size by approximately 81.5% compared to SVGA

### License Requirement
Using the SDK requires applying for the corresponding Gift Animation Effect License. Playback is only available after setting the License via `TCMediaXBase` (Android/iOS) or `FTCMediaXBase` (Flutter).

---

## Usage Guide

Depending on the target platform, refer to the corresponding references file for detailed integration guides and API documentation:

### By Platform

| Platform | References File | Content |
|----------|----------------|---------|
| **Android** | `references/android-integration.md` | SDK integration guide, usage documentation, demo examples, complete code examples |
| **Android** | `references/android-api.md` | Complete API documentation for TCMediaXBase, TCEffectAnimView, TCEffectConfig, etc. |
| **iOS** | `references/ios-integration.md` | SDK integration guide, usage documentation, demo examples, complete code examples |
| **iOS** | `references/ios-api.md` | Complete API documentation for TCMediaXBase, TCEffectAnimView, TCEffectConfig, etc. |
| **Flutter** | `references/flutter-integration.md` | SDK integration guide, usage documentation, demo examples, complete code examples |
| **Flutter** | `references/flutter-api.md` | Complete API documentation for FTCMediaXBase, FTCEffectAnimView, FTCEffectViewController, etc. |

### General Documentation

| References File | Content |
|----------------|---------|
| `references/license-guide.md` | License purchase, binding, renewal, auto-renewal |
| `references/tools-guide.md` | Guide for using the Effect Conversion Tool and Effect Preview Tool |
| `references/faq.md` | FAQ: error code troubleshooting, log extraction, format support, performance optimization |

### Typical Workflow

1. **Confirm Platform** → Read the integration file for the corresponding platform to understand integration steps
2. **Configure License** → Read `references/license-guide.md` to learn about License application and configuration
3. **Implement Playback** → Read the API file for the corresponding platform to find specific interfaces
4. **Fusion Animation** → Look up the fusion animation section (IFetchResource / loadImageForPlayer / FResourceFetcher) in the API file
5. **Troubleshoot Issues** → Read `references/faq.md` to find solutions to common problems on each platform

### Quick Reference to Key Classes

| Class Name | Function | Platform |
|-----------|----------|----------|
| `TCMediaXBase` | SDK base class, License setting, log management | Android / iOS |
| `FTCMediaXBase` | Flutter global management class | Flutter |
| `TCEffectAnimView` | Core player view class | Android / iOS |
| `FTCEffectAnimView` | Flutter playback view component | Flutter |
| `FTCEffectViewController` | Flutter playback controller | Flutter |
| `TCEffectConfig` / `FTCEffectConfig` | Player configuration class | All platforms |
| `TCEffectAnimInfo` / `FTCEffectAnimInfo` | Animation info class | All platforms |
| `TCEffectText` / `FTCEffectText` | Fusion text style class | All platforms |
| `IAnimPlayListener` (Android) | Playback status callback | Android |
| `TCEPAnimViewDelegate` (iOS) | Playback event delegate | iOS |
| `TCEffectPlayerConstant` | Constant definitions (ScaleType, VideoMode) | Android |
