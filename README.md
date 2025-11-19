<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-ngrok.svg?style=flat-square)](https://github.com/bearsampp/module-ngrok/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-ngrok/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving ngrok.

## Documentation and downloads

https://bearsampp.com/module/ngrok

## Build

This project uses Gradle for building and packaging releases.

### Requirements

* Java 8 or higher
* Gradle (or use the Gradle wrapper if available)
* 7-Zip (optional, for `.7z` format; falls back to `.zip` if not available)

### Quick Start

```bash
# List all available tasks
gradle tasks

# Build a specific version interactively
gradle release

# Build a specific version non-interactively
gradle release -PbundleVersion=3.19.1

# Build all available versions
gradle releaseAll

# Display build information
gradle info

# Verify build environment
gradle verify

# List available local versions
gradle listVersions

# List available remote releases
gradle listReleases

# Clean build artifacts
gradle clean
```

### Configuration

Edit `build.properties` to configure the build:

```properties
bundle.name = ngrok
bundle.release = YYYY.MM.DD
bundle.type = tools
bundle.format = 7z
```

### How It Works

1. **Version Detection**: Gradle scans the `bin/` directory for folders matching `bin/ngrok<version>` (e.g., `bin/ngrok3.19.1`)
2. **Binary Download**: If `ngrok.exe` is not present locally, it's downloaded from the [modules-untouched](https://github.com/Bearsampp/modules-untouched) repository
3. **Staging**: Files are staged in `bearsampp-build/tmp/bundles_prep/tools/ngrok/`
4. **Packaging**: The final archive is created in `bearsampp-build/tools/ngrok/<release>/` with hash files (MD5, SHA1, SHA256, SHA512)

### Output

Archives are created with the naming pattern:
```
bearsampp-ngrok-<version>-<release>.<format>
```

For example: `bearsampp-ngrok-3.19.1-2025.02.16.7z`

### Advanced Options

**Override download URL:**
```bash
gradle release -PbundleVersion=3.19.1 -PngrokUrl=https://example.com/ngrok.exe
# or set environment variable
set NGROK_DOWNLOAD_URL=https://example.com/ngrok.exe
```

**Use local ngrok.exe:**
```bash
gradle release -PbundleVersion=3.19.1 -PngrokLocalExe=C:/path/to/ngrok.exe
# or set environment variable
set NGROK_EXE_PATH=C:/path/to/ngrok.exe
```

**Custom build path:**
```bash
# In build.properties
build.path = C:/custom-build-path
# or set environment variable
set BEARSAMPP_BUILD_PATH=C:/custom-build-path
```

### Documentation

For detailed documentation, see the [.gradle-docs](.gradle-docs/) directory.

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
