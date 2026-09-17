**在 Codex 中使用 GPT-6 Astra 氛围编程构建。**

[English](README.md) | [日本語](README.ja.md) | 简体中文

# Folduo

出于好奇我做了这个项目，没有积极开发或维护的打算。如果有什么让我感兴趣，我可能会做些改动，否则这个仓库基本会保持原样。

一个实验性的 Galaxy Z Fold7 应用：利用铰链角度在外屏与内屏之间打造磨砂玻璃式的过渡效果。手机折叠时，它会让应用的画面留在原处，并加上视差和模糊，然后移交到另一块屏幕上的应用。它适用于普通应用，无需替换你的启动器。

[下载 v0.1.21](https://github.com/bunkaich/Folduo/releases/tag/v0.1.21)

## 系统要求

- **仅支持 Galaxy Z Fold7 SM-F966Z。** 其他机型不会启用屏幕控制。
- 已在 Android 16 / One UI 8.5 上测试，版本号 `F966ZSCS1BZH4`。
- [Shizuku](https://shizuku.rikka.app/guide/setup/) 已安装并运行。测试版本为 `13.6.0.r1086.2650830c`。
- 按下文说明配置好的、受支持的三星原厂互动壁纸。

无需 root。完成初始设置后，只要通过无线调试启动 Shizuku，就可以在没有 USB 的情况下运行。设备重启后必须重新启动 Shizuku。无 USB 情况下的长期稳定性尚未验证。

## 设置

Folduo 支持英语、日语和简体中文。在应用顶部点按 **语言 / Language**，可选择 **English**、**日本語**、**简体中文** 或 **跟随系统设置**。选择会被保存，也会显示在 Android 的应用语言设置中。默认情况下，日语设备使用日语，其他设备使用英语。

### 1. 启动 Shizuku

按照[官方设置指南](https://shizuku.rikka.app/guide/setup/)安装 Shizuku，并通过无线调试或电脑启动。

### 2. 配置原厂壁纸

精细角度来自三星互动壁纸，经由 Shizuku 辅助服务获取。在测试设备上，标准铰链传感器主要只报告 0°、90° 和 180°。本应用不会用两个陀螺仪来估算角度。

1. 停止 Folduo，以及其他折叠动画或屏幕控制辅助服务。
2. 将内屏主屏幕设置为三星原厂互动壁纸，其内部标识为 `video_002.mp4`。外屏主屏幕必须使用与之配套的原厂图像 `sub_wallpaper_002`。设置中显示的壁纸名称因系统版本而异。
3. 若想在外屏也获得精细角度，请用下面的辅助工具在外屏应用同一张原厂互动壁纸。**这也会更改 One UI 外屏主屏幕的壁纸。** 如果以后想恢复，请保留你原来的壁纸。

从发布页下载并解压 `folduo-wallpaper-setup-0.1.21.zip`。安装 Python 3 和 Android SDK platform-tools（ADB）。连接一台已授权 USB 调试的手机，然后在解压出的文件夹中运行以下命令：

```sh
python3 cover-wallpaper.py status
python3 cover-wallpaper.py apply
```

`status` 只检查壁纸，不做更改；`apply` 只更改外屏主屏幕壁纸，不涉及锁屏。如果 ADB 不在 PATH 中，请加上 `--adb /path/to/adb`；如果连接了多台设备，请加上 `--serial DEVICE_SERIAL`。

辅助工具会拒绝覆盖不受支持或自定义的壁纸。如果它报告 `other wallpaper` 或 `Expected inner angle-aware wallpaper unavailable`，说明所需壁纸尚未配置。它使用的是手机中已安装的资源；本仓库不包含任何三星壁纸文件。

如需自行构建辅助工具，请先准备[构建环境](#从源码构建)，然后在仓库根目录运行：

```sh
python3 tools/build-wallpaper-helper.py
python3 tools/cover-wallpaper.py status
python3 tools/cover-wallpaper.py apply
```

### 3. 安装并启动应用

1. 从发布页安装 `Folduo-0.1.21.apk`。使用 ADB：`adb install -r Folduo-0.1.21.apk`。
2. 打开 **Folduo**，点按 **连接 Shizuku** 并授予访问权限。
3. 点按 **允许在其他应用上层显示**，并允许通知。
4. 阅读屏幕截取说明，然后点按 **允许临时访问屏幕并启用**。
5. 在手机解锁状态下，将它完整闭合一次以完成初始化。打开计算器等应用，缓慢地折叠和展开手机。

## 操作与限制

外屏使用三星的常规导航。内屏有一个小型的自定义栏，提供 **最近任务、主页、返回和设置**。内屏无法完全使用系统原生的导航手势和通知/快捷设置面板。需要常规操作时，请使用自定义栏、外屏，或停止应用。

- 启用期间两块屏幕都会保持点亮，耗电会增加。停止或锁定后恢复正常屏幕控制。
- 过渡使用的是静止画面。该画面中的视频和游戏不会继续播放。
- 主页、最近任务等系统界面不会按普通应用任务处理。受保护的屏幕和拒绝迁移显示的应用不受支持。
- 应用尺寸变化仍可能导致布局偏移。三星的私有 API 和壁纸响应可能随系统更新而变化。
- 如果 Shizuku 停止，动画也会停止。不保证能自动恢复。

## Folduo 主屏幕

要使用内置的启动器，请在 Folduo 设置中点按 **将 Folduo 设为主屏幕应用** 并选择 Folduo。点按图标可打开应用，长按可替换，也可以使用 **所有应用** 浏览已安装的应用。点按主屏幕上的 **Folduo** 按钮可返回设置。支持英语、日语和简体中文。

在测试的 Fold7 上，三星会把从内屏发起的新应用启动重定向到外屏。Folduo 主屏幕只把所选应用移动到内屏，并在返回时恢复所选的主屏幕。这不会修复其他启动器的同类问题。

在辅助服务保持两块屏幕点亮的手机上，通过了计算器与主屏幕三次往返、主屏幕在两块屏幕之间移动、长按选择和打开设置的测试。包含物理折叠的最终检查仍未完成。如果展开后应用没有打开，请合上手机，从外屏主屏幕启动它。

## 停止与恢复

- **停止：** 打开应用并点按 **停止并释放屏幕控制**。卸载前请先执行此操作。
- **恢复：** 确认 Shizuku 正在运行，然后使用通知中的 **继续** 或应用中的 **继续动画**。解锁并将手机完整闭合一次。
- **重启后：** 再次启动 Shizuku，如有需要再恢复应用。
- **如果屏幕或控制卡住：** 合上手机，从外屏停止应用。如果无法操作，请重启设备，并在重新启动 Shizuku 前先停用应用的“始终启用”模式。
- **恢复壁纸：** 停止应用，然后在 Android 设置中重新选择壁纸。若要恢复辅助工具更改过的特定原厂外屏图像，请在辅助工具的解压目录中运行：

```sh
python3 cover-wallpaper.py restore-stock
```

从源码检出运行时，请使用 `python3 tools/cover-wallpaper.py restore-stock`。它恢复的是已知的原厂图像，而不是任意之前的壁纸。如果设置之后又选择了其他壁纸，它会拒绝覆盖。

## 从源码构建

需要 Git、JDK 17 和 Android SDK。将 `JAVA_HOME` 指向你的 JDK，`ANDROID_HOME` 指向你的 SDK；使用 ADB 时请把 `platform-tools` 加入 PATH。

```sh
sdkmanager "platforms;android-37.0" "build-tools;36.0.0" "platform-tools"
sdkmanager --licenses

git clone https://github.com/bunkaich/Folduo.git
cd Folduo
git checkout v0.1.21
./gradlew :app:assembleRelease :app:testDebugUnitTest :app:lintRelease
```

输出路径：`app/build/outputs/apk/release/app-release.apk`。Windows 上请使用 `gradlew.bat`。构建已在 macOS / Java 17 上验证；Windows 和 Linux 未经端到端验证。

Wrapper 固定使用 Gradle 9.5.1 并校验其校验和。AGP 为 9.2.1；compile SDK 为 37，target SDK 为 36，min SDK 为 33。首次构建需要联网下载依赖。壁纸辅助工具还需要 Python 3。

发布版 APK 使用现有的实验性调试签名证书，签名密钥不公开。你自己构建的版本使用本地证书，无法直接覆盖发布版 APK。切换签名前请先停止并卸载现有应用；设置和权限需要重新配置。卸载不会恢复壁纸。

发布下载中包含 `SHA256SUMS`。可在 macOS 上用 `shasum -a 256 Folduo-0.1.21.apk`，或在 Linux 上用 `sha256sum Folduo-0.1.21.apk` 进行比对。

## 屏幕访问

Shizuku 授予 ADB shell 级别的权限。截取的图像仅在内存中使用，应用不会保存或上传。受保护的屏幕会被排除。本应用没有联网权限，没有分析，也没有广告。请勿在公开 Issue 中发布私人截图、设备序列号或未处理过的诊断日志。

## 许可证

原创代码：[MIT](LICENSE)。依赖库请参见[第三方声明](THIRD_PARTY_NOTICES.md)。本项目不分发任何 Apple 或 Samsung 的界面素材、壁纸或视频。本项目与 Apple、Samsung 或 Shizuku 均无关联。
