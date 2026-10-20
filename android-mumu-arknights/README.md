# Android MuMu Arknights Plugin

A ZCode plugin for building, running, debugging, screenshotting, and UI-automating
Android apps on the **MuMu 12 Arknights edition** at
`D:\VMs\YXArkNights-12.0`.

It is a MuMu-only derivative of the official `android-emulator` plugin. The
device backend is MuMu instance 0 (ADB `127.0.0.1:16384`); AVD management tools
are removed, and `android_start_emulator` launches or reuses MuMu.

## Requirements

- Node.js on `PATH` (the MCP server is `node dist/mcp/server.js`).
- Android SDK with `adb`, build-tools, and a JDK for Gradle builds.
- A Gradle wrapper in the Android project.
- MuMu 12 Arknights edition installed at the configured `mumu_path`.

## Configuration

| option | default | meaning |
|---|---|---|
| `mumu_path` | `D:\VMs\YXArkNights-12.0` | MuMu installation root |
| `mumu_instance` | `0` | MuMu instance index |
| `prefer_mumu` | `true` | Start/reuse MuMu before other targets |
| `sdk_path` | empty | Optional Android SDK root |
| `api_level` | `35` | API level for generated projects |
| `build_tools_version` | `35.0.0` | Build-tools version guidance |
| `jdk_major` | `17` | JDK major version |

## Tools

The plugin keeps the app-development and inspection tools of the original
plugin: preflight, project discovery/creation, build, build-and-run, install,
launch, terminate, open URL, screenshot, logcat, and UI Automator actions.
`android_start_emulator` / `android_start_mumu` start or reuse MuMu;
`android_stop_emulator` stops it through `MuMuManager.exe`.

## Notes

- MuMu provides arm64 via `libnb.so`; normal JVM/Compose apps and x86_64 native
  code run without changes.
- The screenshot tool uses the ADB path. The native MuMu fast-capture API is
  `shell/sdk/external_renderer_ipc.dll` (`nemu_capture_display`), which is
  separate from this plugin.
- AVD creation/listing is intentionally not exposed.
