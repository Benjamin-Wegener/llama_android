# Llama Android

An Android application that wraps the [llama.cpp](https://github.com/ggerganov/llama.cpp) library, specifically providing a native interface to the `llama-server` with Vulkan acceleration using the Android NDK.

## Description

This project integrates llama.cpp as a submodule to create a deployable Android APK. The app serves as a wrapper for running the llama-server binary, optimized for Vulkan GPU acceleration on Android devices that support it. The build process leverages the Android NDK to compile the native C++ code.

### Features
- Native llama-server integration
- Vulkan acceleration for improved performance
- Cross-platform build using CMake and Android NDK
- Support for running large language models on Android

## Prerequisites

- Android Studio (latest version recommended)
- Android NDK (21.4.7075529 or newer)
- CMake (3.22 or newer)
- Git
- A Vulkan-compatible Android device for testing Vulkan features

### Optional
- Vulkan SDK for development and validation

## Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/your-username/llama_android.git
   cd llama_android
   ```

2. Initialize and clone the llama.cpp submodule:
   ```bash
   git submodule init
   git submodule update
   ```

   This will clone [llama.cpp](https://github.com/ggerganov/llama.cpp) into the `llama.cpp/` directory.

3. Ensure you have the NDK installed via Android Studio or standalone.

## Build Instructions

### 1. Build the Android APK

1. Open the project in Android Studio.

2. Configure the Android NDK in Android Studio settings if not automatically detected.

3. Sync the project and build the APK:
   - Press the "Sync Project" button.
   - Select Build > Build Bundle(s)/APK(s) > Build APK(s).

The build automatically enables Vulkan support with `-DGGML_VULKAN=ON` and configures for `arm64-v8a` ABI.

### 2. Build the llama-server Binary (if needed externally)

For testing or manual use, build the llama-server binary separately:
```bash
chmod +x build_server.sh
./build_server.sh
```
This creates the binary in `build_android_server/examples/server/llama-server`.

To use in the app, copy this binary to your device's `/data/local/tmp/` directory (e.g., via ADB).

### Building with Command Line

If building outside Android Studio:
```bash
# Assuming NDK is set up
cd llama_android
# Use Gradle to build
./gradlew assembleDebug
```

## Usage

1. Install the generated APK on a compatible Android device.

2. Copy the `llama-server` binary to your device's `/data/local/tmp/` directory (using ADB push).

3. Copy your model file (e.g., `model.gguf`) to the path specified in `BuildConfig.LLAMA_MODEL_PATH` (default: `/data/local/tmp/llama/model.gguf`).

4. Launch the app and tap "Start Server". The app will:
   - Start the llama-server process in the background
   - Show the HTML interface in a WebView after a brief delay

5. Interact with the llama-server via the embedded web interface at http://127.0.0.1:8080.

6. The server utilizes Vulkan acceleration for GPU inference when available.

## Project Structure

- `app/`: Android application module
  - `build.gradle`: Gradle build configuration with NDK and Vulkan settings
  - `src/main/`: Main source directory
    - `AndroidManifest.xml`: App manifest with permissions
    - `cpp/`: Native C++ sources and CMakeLists.txt
      - `CMakeLists.txt`: CMake configuration for native builds
      - `native-lib.cpp`: JNI implementation linking to llama.cpp
    - `java/`: Java/Kotlin sources (MainActivity)
    - `res/`: Resources (layouts, strings)
- `llama.cpp/`: Submodule for llama.cpp library
- `build_server.sh`: Script to build llama-server binary for Android
- `settings.gradle`: Project settings
- `README.md`: This documentation

## License

This project is licensed under the MIT License. See LICENSE file for details.

The llama.cpp submodule is independently licensed under the MIT License as well.

## Contributing

Contributions are welcome. Please ensure that changes are compatible with the Android build process and respect the Vulkan acceleration requirements.

## Troubleshooting

- **Vulkan not working**: Ensure your device and emulator support Vulkan. Check device compatibility.
- **Build failures**: Verify NDK version and CMake configuration.
- **Submodule issues**: Run `git submodule update --init --recursive` to ensure all dependencies are correct.
