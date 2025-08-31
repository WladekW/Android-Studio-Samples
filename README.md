# Android Studio Samples

Build, install, and run sample Android projects **without Android Studio** using the included Gradle Wrapper (`./gradlew`), `adb`, and standard Android SDK command-line tools.

-----

## Requirements

  - **Java JDK 11+** (verify with `java -version`).
  - **Android SDK command-line tools** and **`platform-tools`** (contains `adb`).
  - Android `build-tools` and the Android platform(s) targeted by the projects.
  - A device with **USB debugging enabled** or an Android emulator.
  - **Gradle Wrapper** (`gradlew` / `gradlew.bat`) — if missing, install Gradle or generate a wrapper.

### Suggested Environment Variables (Linux / macOS)

Add these to your `~/.bashrc` or `~/.zshrc` file:

```bash
export ANDROID_SDK_ROOT="$HOME/Android/Sdk"
export PATH="$ANDROID_SDK_ROOT/platform-tools:$PATH"
```

-----

## Quick Start (Linux / macOS)

1.  **Open terminal** and navigate to the repository root:

    ```bash
    cd /path/to/Android-Studio-Samples
    ```

2.  **Refresh dependencies**:

    ```bash
    ./gradlew --refresh-dependencies
    ```

3.  **Build the project**:

    ```bash
    ./gradlew build
    # or for a faster debug build:
    ./gradlew assembleDebug
    ```

4.  **Check connected devices/emulators**:

    ```bash
    adb devices
    ```

5.  **Install an app** (using `app` as an example module name):

    ```bash
    ./gradlew :app:installDebug
    ```

    *Alternatively, you can install the APK manually:*

    ```bash
    adb install -r app/build/outputs/apk/debug/app-debug.apk
    ```

6.  **Launch the app** (optional):

    ```bash
    adb shell monkey -p com.your.package.name -c android.intent.category.LAUNCHER 1
    # OR
    adb shell am start -n com.your.package.name/.MainActivity
    ```

-----

## Windows (PowerShell / cmd)

Use `gradlew.bat` instead of `./gradlew`:

```powershell
gradlew.bat --refresh-dependencies
gradlew.bat build
adb devices
gradlew.bat :app:installDebug
```

-----

## Build & Install Specific Modules or Variants

### Build a specific module:

```bash
./gradlew :sample1:assembleDebug
```

### Install a specific module:

```bash
./gradlew :sample1:installDebug
```

### Build a specific flavor/variant:

```bash
./gradlew :app:assembleFreeDebug
```

-----

## Emulators (Optional)

### Install platform/system image if needed:

```bash
sdkmanager "platform-tools" "platforms;android-33" "system-images;android-33;google_apis;x86_64"
```

### Create and run an AVD:

```bash
avdmanager create avd -n myAVD -k "system-images;android-33;google_apis;x86_64" --device "pixel"
emulator -avd myAVD
```

### Verify and install to the emulator:

```bash
adb devices
./gradlew :app:installDebug
```

-----

## Multiple Devices / Target by Serial

### Find the device serial:

```bash
adb devices
```

### Install to a specific device:

```bash
adb -s <serial> install -r app/build/outputs/apk/debug/app-debug.apk
```

*Or, set the serial for Gradle (example property usage):*

```bash
./gradlew -Pandroid.testInstrumentationRunnerArguments.deviceSerial=<serial> :app:installDebug
```

-----

## Tests

### Run unit tests (JVM):

```bash
./gradlew test
```

### Run instrumented tests on a device:

```bash
./gradlew connectedAndroidTest
```

-----

## Release Builds & Signing

Configure `signingConfigs` in `app/build.gradle` or via `gradle.properties`.

### Build a release APK:

```bash
./gradlew :app:assembleRelease
# or build an Android App Bundle for the Play Store:
./gradlew :app:bundleRelease
```

### Verify the signed APK (requires `apksigner` from `build-tools`):

```bash
apksigner verify app/build/outputs/apk/release/app-release.apk
```

-----

## Troubleshooting

  - **`adb devices` is empty:**

      - Enable **Developer options** → **USB debugging** on your device.
      - Approve the debugging prompt on your device.
      - Install vendor USB drivers on Windows.

  - **Gradle / Java version mismatch:**

      - Ensure you're using the JDK version required by the Android Gradle Plugin (JDK 11+ is typical).

  - **Wrong target device when multiple devices are connected:**

      - Use `adb -s <serial>` or disconnect other devices.

  - **Dependency/build errors:**

      - Try a clean build with a dependency refresh:

    <!-- end list -->

    ```bash
    ./gradlew clean
    ./gradlew --refresh-dependencies build
    ```

-----

## Useful Commands (Cheat Sheet)

```bash
./gradlew --refresh-dependencies    # refresh dependencies
./gradlew build                     # full build
./gradlew assembleDebug             # create debug APK(s)
./gradlew :module:assembleDebug     # build a specific module
./gradlew :app:installDebug         # build + install
adb devices                         # list connected devices
adb install -r path/to/app-debug.apk# manual install
adb uninstall com.your.package      # uninstall an app
./gradlew test                      # run unit tests
./gradlew connectedAndroidTest      # run instrumented tests
```

-----

## Customize for this Repository

To create a tailored README that exactly matches your module names, paste the contents of `settings.gradle`, `settings.gradle.kts`, or a top-level folder list, and I'll adapt this file.

I can also create small helper scripts (Bash / PowerShell) to automate the `build` → `uninstall` → `install` process for a chosen module.

-----

## License & Contributing

*Add a `LICENSE` file if you want to state the project license.*

To add a new sample: create a new Gradle module, add it to `settings.gradle`, and document its build/install process here.
