---
name: android-mumu-dev
description: Build, run, inspect, and automate Android apps on the MuMu 12 Arknights edition.
---

# Android Dev on MuMu

Use this skill to create, build, run, debug, screenshot, or automate an Android app on the MuMu 12 Arknights edition (`D:\VMs\YXArkNights-12.0`). The MCP server is `android-mumu`; tools are exposed as `mcp__android_mumu__<tool>`.

## Default Workflow

1. Call `mcp__android_mumu__android_preflight` first.
   - The SDK, adb, Java, and Gradle wrapper are needed for builds.
   - AVD/emulator checks are optional because the device backend is MuMu.
   - If setup is missing, follow `skills/android-mumu-dev/INSTALL_ENVIRONMENT.md`.
2. Discover the project with `mcp__android_mumu__android_discover_project`.
   - If no project exists, call `mcp__android_mumu__android_create_app`.
3. Build and launch with `mcp__android_mumu__android_build_and_run`.
   - With `prefer_mumu=true` (default), it starts or reuses MuMu automatically.
   - Alternatively call `mcp__android_mumu__android_start_emulator` (or `android_start_mumu`) first; it returns the serial, normally `127.0.0.1:16384`.
   - Pass `module`, `variant`, or `applicationId` when discovery is ambiguous.
4. Verify visually with `mcp__android_mumu__android_screenshot`.
5. Use `android_logs`, `android_open_url`, `android_launch_app`, and `android_terminate_app` for runtime checks.
6. For UI automation, call `android_ui_status` first, then `android_ui_describe` / `android_ui_resolve`, then tap/swipe/type/keyevent.

## MuMu Backend

- MuMu installation: `D:\VMs\YXArkNights-12.0`, instance `0`, ADB port `16384`.
- `android_start_emulator` is the MuMu start operation in this plugin; it reuses an already running instance.
- `android_stop_emulator` stops MuMu through `MuMuManager.exe`.
- `android_list_devices` reports MuMu as an emulator target.
- AVD creation/listing tools are intentionally omitted because MuMu instances are managed by MuMuManager.
- MuMu path, instance, and preference are configured with `mumu_path`, `mumu_instance`, and `prefer_mumu`.
- MuMu uses its `libnb.so` native bridge for ARM-only APKs. Normal Java/Kotlin/Compose apps and x86_64 native code run directly; ARM-only native libraries are translated by MuMu.
- The screenshot tool uses the ADB path. For the low-latency native MuMu capture API, use the separate `external_renderer_ipc.dll` integration outside this plugin.

## Tool Notes

- Keep emulator interactions through MCP tools instead of raw adb/emulator commands unless a tool does not cover the operation.
- `android_build_app` only builds. `android_build_and_run` builds, reuses/starts MuMu, installs, and launches.
- Do not use `android_create_avd`; it does not exist in this plugin.
- The Android SDK path, API level, build-tools version, JDK major version, and MuMu settings come from plugin user config.

## Project Requirements

Before building, ensure the project has:

- `settings.gradle` or `settings.gradle.kts`
- root `build.gradle` or `build.gradle.kts`
- `app/build.gradle` or `app/build.gradle.kts`
- `gradle.properties` with `android.useAndroidX=true`
- `local.properties` with `sdk.dir=<Android SDK path>` when the SDK is not discoverable
- a Gradle wrapper (`gradlew` / `gradlew.bat`) or `gradle` on `PATH`

## Troubleshooting

- If `android_preflight` reports `Gradle` as `not found`, add a Gradle wrapper to the project or install Gradle.
- If MuMu cannot be reached, confirm instance 0 is running and `adb connect 127.0.0.1:16384` succeeds.
- If the screenshot or UI tools fail on MuMu, rerun the tool; MuMu's UI dump can return a non-zero exit code even after writing the XML, which this plugin tolerates.
- If the app installs but cannot start, verify its ABIs. MuMu provides `arm64-v8a` through `libnb.so`; pure x86_64 apps run natively.
