# Bearsampp Module Ngrok — Gradle Documentation

This directory contains documentation for the Gradle-based build system for the Bearsampp Ngrok module.

## Contents

- [Usage](./usage.md) — Common commands, tasks, and build examples

## Overview

The Ngrok module uses Gradle to automate the download, staging, and packaging of ngrok releases for Bearsampp. The build system:

- Automatically detects available versions from the `bin/` directory
- Downloads `ngrok.exe` from the [modules-untouched](https://github.com/Bearsampp/modules-untouched) repository when needed
- Stages files in a shared `bearsampp-build/tmp` directory structure
- Creates release archives with hash files (MD5, SHA1, SHA256, SHA512)
- Supports both interactive and non-interactive builds

## Key Features

- **Version Resolution**: Automatically scans `bin/` and `bin/archived/` directories for available versions
- **Smart Downloads**: Fetches binaries from modules-untouched with fallback URL construction
- **Flexible Configuration**: Supports custom build paths, download URLs, and local binaries
- **Hash Generation**: Automatically generates MD5, SHA1, SHA256, and SHA512 hash files
- **Multiple Formats**: Supports both 7z and zip archive formats

## Quick Reference

```bash
# Interactive build (select version from menu)
gradle release

# Build specific version
gradle release -PbundleVersion=3.19.1

# Build all versions
gradle releaseAll

# List available versions
gradle listVersions

# List remote releases
gradle listReleases

# Verify environment
gradle verify

# Display build info
gradle info
```

## Configuration

The build is configured via `build.properties`:

```properties
bundle.name = ngrok
bundle.release = YYYY.MM.DD
bundle.type = tools
bundle.format = 7z
```

## Build Output

Archives are created in:
```
bearsampp-build/tools/ngrok/<release>/bearsampp-ngrok-<version>-<release>.<format>
```

Staging directories:
- `bearsampp-build/tmp/bundles_prep/tools/ngrok/` — Prepared files for archiving
- `bearsampp-build/tmp/bundles_build/tools/ngrok/` — Non-archived copy for inspection

## Requirements

- Java 8 or higher
- Gradle
- 7-Zip (optional, for `.7z` format)

For more details, see [Usage](./usage.md).
