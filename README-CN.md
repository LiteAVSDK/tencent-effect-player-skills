简体中文 | [English](./README.md)

# Tencent Gift Effect SDK Skills 使用指引

Tencent Gift Effect SDK Skills 为腾讯礼物动画特效 SDK 播放场景提供结构化指引。它可以将自然语言需求转化为可执行步骤，帮助 AI Agent 高效完成礼物动画特效 SDK 在 Android / iOS / Flutter 平台的集成。

## 前置条件

使用 Skills 前，请确保您的 AI 编程工具已支持 Skills 功能。目前支持的工具包括：
- CodeBuddy

- Trae

- Cursor

- Codex

- Claude Code

- Claude Desktop


## 安装

### 方式一：手动下载后导入

[点击这里](https://mediacloud-76607.gzc.vod.tencent-cloud.com/aicoding/skills/tencent-vod-player-skills.zip) 下载 Tencent Gift Effect SDK Skills 安装 zip 包，解压后导入到 AI 编程工具的 Skills 存放目录。

### 方式二：通过 GitHub 克隆安装

可通过 GitHub 克隆：
``` bash
git clone https://github.com/LiteAVSDK/tencent-effect-player-skills ~/.skills/tencent-gift-effect-skills
```

## 优势

|优势|说明|
|---------|---------|
|**结构化知识注入**|将腾讯礼物动画特效 SDK 领域知识直接注入 Agent 上下文，降低歧义，提升输出一致性|
|**与 Prompt 工程深度配合**|内置标准化执行流程，帮助 Agent 按可复用路径完成礼物动画特效 SDK 集成|
|**零运行时依赖**|Skills 为静态文本资源，无需额外启动服务|
|**全平台覆盖**|单一技能包覆盖 Android / iOS / Flutter 三大平台的礼物动画特效集成|
|**场景全面**|覆盖普通礼物特效、融合动画、头像框动画、全屏大礼物、多动画同屏播放、MP4 透明通道动画等丰富场景|


## 使用示例

安装 Skills 后，您可以直接向 AI Agent 描述需求，Agent 将自动调用 Skills 中的结构化知识完成任务：

### 示例 1：集成礼物特效播放器
``` bash
请帮我在 Android 项目中集成礼物动画特效SDK，实现 MP4 礼物动画播放功能
```

### 示例 2：实现融合动画
``` bash
我需要在 iOS 应用中实现带用户头像和昵称的融合动画礼物效果
```

### 示例 3：多动画同屏场景
``` bash
使用 TCEffectAnimView 实现直播间 40+ 头像框动画与全屏大礼物同屏流畅播放
```

### 示例 4：动画格式转换
``` bash
我有一批 SVGA 和 PAG 动画素材，如何使用特效转换工具转成 TCMP4 格式以减小体积？
```

### 示例 5：多平台对比
``` bash
对比 Android 和 iOS 平台的礼物特效播放器 API 差异，列出主要的类和方法
```

## 触发关键词

当您的提问包含以下关键词时，AI Agent 会自动激活 Gift Effect SDK Skills：
- 中文关键词：`腾讯礼物动画特效 SDK`、`礼物播放器`、`特效播放器`、`礼物动画`、`动画特效`、`融合动画`、`特效转换工具`、`特效预览工具`、`礼物特效 License`、`头像框动画`、`全屏礼物`、`MP4 透明通道`

- 英文关键词：`Gift AR`、`TCEffectAnimView`、`TCEffectPlayer`、`TCMediaXBase`、`TCEffectConfig`、`TCEffectAnimInfo`、`TCEffectText`、`FTCEffectAnimView`、`FTCMediaXBase`、`FTCEffectViewController`、`TEP`、`TCMP4`、`VAP`


## Skills 内容结构
``` bash
tencent-gift-effect-skills/
├── SKILL.md                              # Skills 主文件
├── LICENSE.txt                           # 开源协议
└── references/                           # 参考文档
    ├── android-integration.md            # Android 集成指南
    ├── android-api.md                    # Android API 文档
    ├── ios-integration.md                # iOS 集成指南
    ├── ios-api.md                        # iOS API 文档
    ├── flutter-integration.md            # Flutter 集成指南
    ├── flutter-api.md                    # Flutter API 文档
    ├── license-guide.md                  # License 配置指南
    ├── tools-guide.md                    # 特效转换工具与预览工具使用指南
    └── faq.md                            # 常见问题
```

## 参考
- [Tencent Gift Effect SDK Skills (GitHub)](https://github.com/LiteAVSDK/tencent-effect-player-skills)

- [腾讯礼物动画特效 SDK 官方文档](https://cloud.tencent.com/document/product/616/116459)


