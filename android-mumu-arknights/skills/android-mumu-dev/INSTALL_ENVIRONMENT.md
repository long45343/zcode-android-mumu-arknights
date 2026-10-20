# MuMu Android Development Environment

This plugin builds and runs Android apps on the **MuMu 12 Arknights edition**.
MuMu provides the Android device; the Android SDK and JDK are needed for Gradle
builds, and Node.js runs the MCP server. No AVD is required.

## Required

- Android SDK with `adb` (platform-tools), build-tools, and a platform
  (`platforms;android-35` or the project's compileSdk).
- JDK 17 (or the project's required JDK).
- Gradle wrapper in the Android project (`gradlew` / `gradlew.bat`).
- Node.js on `PATH`.
- MuMu 12 Arknights edition installed at
  `D:\VMs\YXArkNights-12.0` (instance 0).

## Configure the SDK

Set the plugin option `sdk_path` to the Android SDK root, or set
`ANDROID_HOME` / `ANDROID_SDK_ROOT`. Ensure these exist:

```
<SDK>\platform-tools\adb.exe
<SDK>\cmdline-tools\latest\bin\sdkmanager.bat
<SDK>\build-tools\<version>\
```

## Connect MuMu

Start the MuMu instance, then:

```bat
adb connect 127.0.0.1:16384
adb devices
```

The expected target is `127.0.0.1:16384  device`. The plugin also starts the
instance automatically through `MuMuManager.exe` when needed.

## Check

Call `android_preflight` and resolve any missing SDK, Java, or Gradle checks.
AVD/emulator checks do not apply to this plugin; MuMu is the device backend.
