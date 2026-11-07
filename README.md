# RoomSiege · 房间防线 — Vibe Lab 提交工程

让你的房间，成为战场。在真实房间中部署全息防御武器，搭配减速、爆破和辅助道具，守护核心并抵御十波敌人。支持模拟沙盘、局外商店、图鉴、三档难度及本地进度。

本仓库用于 **PICO Vibe Lab 的 GitHub 云打包**。它包含 Unity 导出的 Android Gradle 工程、正式编译的 ARM64 IL2CPP 游戏库、场景和资源。可编辑的完整 Unity C# 工程在 https://github.com/ultimetamr/RoomSiege 。

## 提交与构建

Vibe Lab 中填写本仓库 URL 与完整 40 位 Commit ID，构建目录为仓库根目录。不要选择只有 `Assets/Packages/ProjectSettings` 的 Unity 源码仓库执行 Gradle。

```sh
gradle --no-daemon --console=plain assembleRelease
```

输出：`launcher/build/outputs/apk/release/launcher-release.apk`。

- Unity **6000.6.8f1**，Release，IL2CPP，ARM64。
- Android Gradle Plugin **8.10.1**，Gradle **8.11.1**，Java **17**。
- Android compile/target SDK **36**，Build Tools **36.0.0**，min SDK **29**。
- 应用包名 **com.roomsiege.mr**；版本 **0.1.0**，versionCode **1**，平台可覆盖构建号。
- 自定义 RoomSiege 图标；作品提交图标为 `Docs/Media/RoomSiege-Icon-1024.png`。

选择 AGP 8.10.1 是为了匹配当前云端日志中的 Gradle 8.11.1，同时支持 API 36。版本依据：https://developer.android.com/build/releases/past-releases/agp-8-10-0-release-notes 。使用与 Unity 导出一致的 Build Tools 36.0.0，未绑定开发机的路径。

构建机需要 Android SDK 36、对应 Android 构建工具与 Java 17。通过 `ANDROID_HOME` 或构建机自己的 `local.properties` 指定 SDK；无需安装 Unity、Windows IL2CPP 工具或本机 NDK。原生库使用 Unity 匹配的 NDK 正式编译后纳入提交，Gradle 只负责 Java、资源与 APK 封装，不生成假库。

保留 Unity 模板的 debug signingConfig 供本地可安装构建使用；没有提交任何私钥或密码。平台发行签名由平台流程处理，覆盖安装还取决于签名一致性。

## 与 Unity 源码的关系

本仓库不能自动编译后来修改的 C#。更新游戏后，必须重新执行 Unity Release 导出和原生编译，再通过源码仓库的 `Tools/Prepare-VibeLab.py` 生成新的提交工程。

`build-provenance.json` 记录源代码 Commit、正式编译 APK 的哈希和关键运行库/元数据/图标的 SHA256。每次构建先检查这些输入，防止遗漏或混用 IL2CPP 库与元数据。未使用 Git LFS，运行库和资源是实际文件。

## 验证边界与许可

构建检查结果见 `Validation/build-validation.json`。本地 Gradle Release 构建通过不等于 Vibe Lab 云端任务或审核已通过；本次没有新增真机游玩验收。房间自动恢复修复的真机复测仍待完成。

本地使用仅包含 SDK36 的独立 SDK 视图验证，未改动全局 SDK：旧版 lint 无法解析同机安装的 `android-37.0` 预览目录。如果构建机也装有这种目录，请使用只包含所需正式 SDK 的构建环境；不需要关闭 lint。现有 PICO 音频运行库的 16KB ELF 对齐警告仍保留，本次未改第三方二进制。

本仓库包含应用运行所需的 Unity/PICO 等第三方编译组件，其权利和使用条件归各自许可方；不包含完整第三方 SDK 源码，不授予独立提取或再分发这些组件的许可。项目源码与资源许可说明见 Unity 源工程。未提交缓存、日志、用户房间数据、存档、`local.properties`、密钥或 APK。
