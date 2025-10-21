# IDE Setup Guide

This guide helps you configure your IDE to properly run the Piccolo Editor.

## CLion Configuration

### Problem
When running `PiccoloEditor` directly from CLion, you may encounter errors like:
```
[error] [Piccolo::AssetManager::loadAsset] open file: .../asset/global/particle.global.json failed!
[error] [Piccolo::AssetManager::loadAsset] open file: .../asset/global/rendering.global.json failed!
```

This occurs because CLion may run the executable from a different working directory than expected.

### Solution

The CMakeLists.txt has been configured to automatically set the working directory for Visual Studio. For CLion, you may need to manually configure the run configuration:

1. **Open Run/Debug Configurations**
   - Click on the run configuration dropdown in the toolbar
   - Select "Edit Configurations..."

2. **Configure PiccoloEditor**
   - Find the `PiccoloEditor` target
   - In the "Working directory" field, ensure it's set to `$<TARGET_FILE_DIR:PiccoloEditor>`
   - Or manually set it to the directory containing the executable (usually `<build_dir>/engine/source/editor/`)

3. **Alternative: Use the Deployment Build**
   - Build the project normally
   - Run the executable from the `bin` directory: `<project_root>/bin/PiccoloEditor.exe`
   - This uses the deployment configuration which expects assets in the current directory

### How It Works

The editor looks for `PiccoloEditor.ini` in the same directory as the executable. There are two configurations:

- **Development** (`engine/configs/development/PiccoloEditor.ini`):
  - Copied to the build directory alongside the executable
  - Uses `BinaryRootFolder=../../../../../bin` to point to the shared bin directory
  
- **Deployment** (`engine/configs/deployment/PiccoloEditor.ini`):
  - Copied to the `bin` directory
  - Uses `BinaryRootFolder=.` for a self-contained deployment

When running from the IDE, ensure the working directory is set to where the executable and its `.ini` file are located.

## Visual Studio Configuration

Visual Studio users should have the working directory automatically configured via the CMakeLists.txt `VS_DEBUGGER_WORKING_DIRECTORY` property.

## Other IDEs

For other IDEs, ensure the working directory is set to the directory containing both:
1. The `PiccoloEditor` executable
2. The `PiccoloEditor.ini` configuration file

This is typically: `<build_directory>/engine/source/editor/`
