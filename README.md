English| [简体中文](./README-CN.md)

# Gift AR SDK Skills User Guide

Gift AR SDK Skills provides structured guidance for the Gift AR SDK playback scenarios. It translates natural language requirements into executable steps, assisting AI Agents in efficiently integrating the Gift AR SDK across Android, iOS, and Flutter platforms.

## Prerequisites

Before utilizing Skills, ensure your AI programming tool supports the Skills functionality. Currently supported tools include:
- CodeBuddy

- Trae

- Cursor

- Codex

- Claude Code

- Claude Desktop


## Installation

### Method 1: Manual Download and Import

[Click here](https://mediacloud-76607.gzc.vod.tencent-cloud.com/aicoding/skills/tencent-vod-player-skills.zip) to download the Gift AR SDK Skills installation package. After extraction, import it into the designated Skills directory of the AI programming tool.

### Method 2: Installation via GitHub Clone

You may also clone the repository from GitHub:
``` bash
git clone https://github.com/LiteAVSDK/tencent-effect-player-skills ~/.skills/tencent-gift-effect-skills
```

## Advantages

|Advantage|Description|
|---------|---------|
|**Structured Knowledge Injection**|Directly infuses domain expertise of the Gift AR SDK into the Agent's context, reducing ambiguity and enhancing output consistency.|
|**Deep Integration with Prompt Engineering**|Features built-in standardized execution workflows, enabling the Agent to complete Gift AR SDK integration through reusable pathways.|
|**Zero Runtime Dependencies**|Skills are static text resources, eliminating the need for additional service initialization.|
|**Cross-Platform Compatibility**|A single skill package supports gift animation effect integration across Android, iOS, and Flutter platforms.|
|**Comprehensive Scenario Coverage**|Addresses diverse scenarios including standard gift effects, blended animations, avatar frame animations, full-screen gifts, concurrent multi-animation playback, and MP4 alpha-channel animations.|


## Usage Example

After installing Skills, you can directly describe your requirements to the AI Agent, which will automatically leverage the structured knowledge within Skills to accomplish tasks:

### Example 1: Integrating the Gift AR Player
``` bash
Please assist me in integrating the Gift AR SDK into an Android project to implement MP4 gift animation playback functionality.  
```

### Example 2: Implementing Blended Animations
``` bash
I need to achieve a composite animation gift effect with user tcmp4 avatars and nicknames in an iOS application.  
```

### Example 3: Multi-Animation On-Screen Scenario
``` bash
Utilize TCEffectAnimView to enable smooth simultaneous playback of 40+ avatar tcmp4 frame animations and full-screen mp4 gift effects in live streaming rooms.  
```

### Example 4: Animation Format Conversion
``` bash
I have a collection of SVGA and PAG animation assets—how can I use the effect conversion tool to transform them into TCMP4 format for reduced file size?  
```

### Example 5: Multi-Platform Comparison
``` bash
Compare the API differences of the Gift AR player between Android and iOS platforms, listing the key classes and methods.
```

## Trigger Keywords

When your inquiry includes any of the following keywords, the AI Agent will automatically activate the Gift AR SDK Skills:


- Chinese Keywords：`腾讯礼物动画特效 SDK`、`礼物播放器`、`特效播放器`、`礼物动画`、`动画特效`、`融合动画`、`特效转换工具`、`特效预览工具`、`礼物特效License`、`头像框动画`、`全屏礼物`、`MP4 透明通道`

- 英文关键词：`Gift AR`、`TCEffectAnimView`、`TCEffectPlayer`、`TCMediaXBase`、`TCEffectConfig`、`TCEffectAnimInfo`、`TCEffectText`、`FTCEffectAnimView`、`FTCMediaXBase`、`FTCEffectViewController`、`TEP`、`TCMP4`、`VAP`


English keywords


## Skills Content Structure
``` bash
tencent-gift-effect-skills/
├── SKILL.md                              
├── LICENSE.txt                           
└── references/                           
    ├── android-integration.md            
    ├── android-api.md                   
    ├── ios-integration.md                
    ├── ios-api.md                        
    ├── flutter-integration.md            
    ├── flutter-api.md                   
    ├── license-guide.md                  
    ├── tools-guide.md                    
    └── faq.md                            
```

## 参考
- [Gift AR SDK Skills (GitHub)](https://github.com/LiteAVSDK/tencent-effect-player-skills)


