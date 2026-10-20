# zcode-android-mumu-arknights

ZCode 插件：在 **MuMu 12 明日方舟专版** 上构建、运行、调试、截图和 UI 自动化 Android 应用。

这是官方 `android-emulator` 插件的 MuMu 专版派生。设备后端改为 MuMu 12 方舟专版（默认实例 0，ADB `127.0.0.1:16384`），保留原有的项目发现、创建、Gradle 构建、安装、启动、截图、logcat 和 UI Automator 工具。

## 仓库结构

```
marketplace.json                 ZCode 本地市场清单
android-mumu-arknights/           插件源码
  .zcode-plugin/plugin.json       插件清单
  .mcp.json                       MCP server 配置（android-mumu）
  dist/                           预构建 MCP server
  skills/android-mumu-dev/        技能与 SDK 环境说明
  commands/                       斜杠命令
  templates/                      新建 Compose 项目模板
```

## 安装

1. Clone 或下载本仓库到本机目录。
2. 打开 ZCode 的 **插件市场 / Plugin Marketplace**。
3. **添加 / Add → 添加插件市场 / Add Plugin Marketplace**，选择包含 `marketplace.json` 的仓库根目录。
4. 在 **个人 / Personal → zcode-android-mumu-arknights** 中找到 **MuMu 方舟 Android 开发**，点击 **安装 / Install**。
5. 安装后可在 **设置 → 插件 / Settings → Plugins** 管理和配置。

插件 ID：`android-mumu-arknights@zcode-android-mumu-arknights`，版本 `0.1.0`。

## 要求

- Node.js 在 `PATH` 上（MCP server 以 `node dist/mcp/server.js` 启动）。
- Android SDK：`adb`、build-tools、platform；JDK 17（或项目要求的版本）。
- Android 项目中有 Gradle wrapper（`gradlew` / `gradlew.bat`）。
- MuMu 12 明日方舟专版，默认路径 `D:\VMs\YXArkNights-12.0`。

## 配置

| 选项 | 默认值 | 说明 |
|---|---|---|
| `mumu_path` | `D:\VMs\YXArkNights-12.0` | MuMu 安装根目录 |
| `mumu_instance` | `0` | MuMu 实例序号 |
| `prefer_mumu` | `true` | 无 serial 时优先启动/复用 MuMu |
| `sdk_path` | 空 | 可选 Android SDK 根目录 |
| `api_level` | `35` | 生成项目使用的 API level |
| `build_tools_version` | `35.0.0` | build-tools 版本提示 |
| `jdk_major` | `17` | JDK 主版本 |

如果 MuMu 安装在其他位置，修改 `mumu_path` 即可。

## 工具

保留原插件的主要开发工具：

- `android_preflight`、`android_discover_project`、`android_create_app`
- `android_build_app`、`android_build_and_run`
- `android_install_app`、`android_launch_app`、`android_terminate_app`、`android_open_url`
- `android_screenshot`、`android_logs`
- `android_ui_status` / `describe` / `resolve` / `tap` / `swipe` / `type_text` / `keyevent`

MuMu 相关：

- `android_start_emulator`：启动或复用 MuMu 实例，返回 serial；`android_start_mumu` 是同一功能的别名。
- `android_stop_emulator`：通过 `MuMuManager.exe` 关闭 MuMu。
- `android_list_devices`：MuMu 显示为 emulator。
- AVD 专用的 `android_create_avd` / `android_list_avds` 已移除；MuMu 实例由 MuMuManager 管理。

## 说明

- 方舟专版使用 `libnb.so` 提供 ARM 翻译；普通 JVM/Compose 应用和 x86_64 原生代码可直接运行，ARM-only 原生库由 MuMu 翻译。
- 插件截图使用 ADB 路径。MuMu 的低延迟原生截图接口是 `shell/sdk/external_renderer_ipc.dll` 的 `nemu_capture_display`，本插件没有接入；需要时可单独集成。
- MuMu 的 `uiautomator dump` 可能返回非 0 退出码但已写出 XML，本插件已兼容。
- 默认路径和实例按方舟专版配置；其他 MuMu 安装请改 `mumu_path` / `mumu_instance`。

## 来源与许可

本项目派生自 Z.ai 的 ZCode `android-emulator` 插件（MIT）。MuMu 后端和工具调整为本地修改。详见 [LICENSE](LICENSE) 和 [NOTICE](NOTICE)。
