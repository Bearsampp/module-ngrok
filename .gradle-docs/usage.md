# Usage

This document describes the available Gradle tasks and common usage patterns for building the Bearsampp Ngrok module.

## Available Tasks

### Build Tasks

#### `gradle release`
Build a release package for a specific version.

**Interactive mode** (prompts for version selection):
```bash
gradle release
```

**Non-interactive mode** (specify version):
```bash
gradle release -PbundleVersion=3.19.1
```

**With custom download URL**:
```bash
gradle release -PbundleVersion=3.19.1 -PngrokUrl=https://example.com/ngrok.exe
```

**With local binary**:
```bash
gradle release -PbundleVersion=3.19.1 -PngrokLocalExe=C:/path/to/ngrok.exe
```

#### `gradle releaseAll`
Build release packages for all available versions found in the `bin/` directory. This task stages all versions but does not create archives (for speed).

```bash
gradle releaseAll
```

#### `gradle clean`
Clean build artifacts and temporary files.

```bash
gradle clean
```

### Information Tasks

#### `gradle info`
Display comprehensive build configuration information including paths, bundle properties, Java version, and Gradle version.

```bash
gradle info
```

#### `gradle listVersions`
List all available ngrok versions found in `bin/` and `bin/archived/` directories.

```bash
gradle listVersions
```

#### `gradle listReleases`
List all available ngrok releases from the modules-untouched repository.

```bash
gradle listReleases
```

#### `gradle tasks`
List all available Gradle tasks.

```bash
gradle tasks
```

### Verification Tasks

#### `gradle verify`
Verify the build environment and dependencies (Java version, required directories, 7-Zip availability, etc.).

```bash
gradle verify
```

#### `gradle validateProperties`
Validate the `build.properties` configuration file.

```bash
gradle validateProperties
```

#### `gradle checkModulesUntouched`
Check the modules-untouched repository integration and list available versions.

```bash
gradle checkModulesUntouched
```

## Build Behavior

### Version Detection
The build system automatically scans for available versions by looking for directories matching the pattern `bin/ngrok<version>` (e.g., `bin/ngrok3.19.1`, `bin/ngrok3`). It also checks the `bin/archived/` directory for archived versions.

### Binary Download
If `ngrok.exe` is not present in the local `bin/ngrok<version>` folder, the build will:

1. Fetch the `ngrok.properties` file from the modules-untouched repository
2. Look up the download URL for the specified version
3. Try fallback URL patterns if the version is not found in properties
4. Download the binary to `bearsampp-build/tmp/downloads/ngrok/`
5. Extract if necessary (supports `.zip`, `.7z`, and `.exe` formats)

### Staging
Files are staged in two locations:

- **Prep directory**: `bearsampp-build/tmp/bundles_prep/tools/ngrok/ngrok<version>/`
  - Used for creating the final archive
  - Contains the version folder at the root (e.g., `ngrok3.19.1/`)

- **Build directory**: `bearsampp-build/tmp/bundles_build/tools/ngrok/ngrok<version>/`
  - Non-archived copy for quick inspection
  - Useful for verifying the build without extracting archives

### Archive Creation
The final archive is created in:
```
bearsampp-build/tools/ngrok/<bundle.release>/bearsampp-ngrok-<version>-<release>.<format>
```

For example:
```
bearsampp-build/tools/ngrok/2025.02.16/bearsampp-ngrok-3.19.1-2025.02.16.7z
```

### Hash Files
Four hash files are automatically generated alongside each archive:
- `.md5` — MD5 hash
- `.sha1` — SHA-1 hash
- `.sha256` — SHA-256 hash
- `.sha512` — SHA-512 hash

## Configuration

### build.properties
The main configuration file for the build:

```properties
bundle.name = ngrok
bundle.release = 2025.02.16
bundle.type = tools
bundle.format = 7z
```

**Properties:**
- `bundle.name` — Module name (should be `ngrok`)
- `bundle.release` — Release date in `YYYY.MM.DD` format
- `bundle.type` — Bundle type (should be `tools`)
- `bundle.format` — Archive format (`7z` or `zip`)

### Environment Variables

**BEARSAMPP_BUILD_PATH**
Override the default build path:
```bash
set BEARSAMPP_BUILD_PATH=C:/custom-build-path
gradle release -PbundleVersion=3.19.1
```

**NGROK_DOWNLOAD_URL**
Override the download URL for ngrok.exe:
```bash
set NGROK_DOWNLOAD_URL=https://example.com/ngrok.exe
gradle release -PbundleVersion=3.19.1
```

**NGROK_EXE_PATH**
Use a local ngrok.exe file instead of downloading:
```bash
set NGROK_EXE_PATH=C:/path/to/ngrok.exe
gradle release -PbundleVersion=3.19.1
```

**7Z_HOME**
Specify the 7-Zip installation directory:
```bash
set 7Z_HOME=C:/Program Files/7-Zip
gradle release -PbundleVersion=3.19.1
```

### Project Properties

Properties can also be passed via command line using `-P`:

```bash
# Specify version
gradle release -PbundleVersion=3.19.1

# Override download URL
gradle release -PbundleVersion=3.19.1 -PngrokUrl=https://example.com/ngrok.exe

# Use local binary
gradle release -PbundleVersion=3.19.1 -PngrokLocalExe=C:/path/to/ngrok.exe
```

## Examples

### Build Latest Version Interactively
```bash
gradle release
# Select version from the menu
```

### Build Specific Version
```bash
gradle release -PbundleVersion=3.19.1
```

### Build with Custom Download URL
```bash
gradle release -PbundleVersion=2.2.8 -PngrokUrl=https://github.com/Bearsampp/modules-untouched/releases/download/ngrok-r1/ngrok_windows_amd64_2.2.8.zip
```

### Build Using Local Binary
```bash
gradle release -PbundleVersion=3.19.1 -PngrokLocalExe=C:/Downloads/ngrok.exe
```

### Build All Versions
```bash
gradle releaseAll
```

### Verify Environment Before Building
```bash
gradle verify
gradle release -PbundleVersion=3.19.1
```

### Check Available Versions
```bash
# List local versions
gradle listVersions

# List remote releases
gradle listReleases
```

### Clean and Rebuild
```bash
gradle clean
gradle release -PbundleVersion=3.19.1
```

## Troubleshooting

### 7-Zip Not Found
If you get an error about 7-Zip not being found:
1. Install 7-Zip from https://www.7-zip.org/
2. Set the `7Z_HOME` environment variable, or
3. Change `bundle.format` to `zip` in `build.properties`

### Download Failures
If downloads fail:
1. Check your internet connection
2. Verify the version exists in modules-untouched: `gradle listReleases`
3. Try with a custom URL: `gradle release -PbundleVersion=X.X.X -PngrokUrl=<URL>`
4. Use a local binary: `gradle release -PbundleVersion=X.X.X -PngrokLocalExe=<PATH>`

### Version Not Found
If a version is not found in `bin/`:
1. Check the directory structure: `bin/ngrok<version>/` (e.g., `bin/ngrok3.19.1/`)
2. List available versions: `gradle listVersions`
3. Ensure the folder name matches the pattern `ngrok<version>`

### Build Path Issues
If you encounter path-related errors:
1. Check that the `dev` project exists in the parent directory
2. Verify `build.properties` is properly configured
3. Try setting a custom build path: `set BEARSAMPP_BUILD_PATH=C:/custom-path`
