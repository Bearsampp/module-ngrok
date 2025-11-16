<p align="center"><a href="https://bearsampp.com/contribute" target="_blank"><img width="250" src="img/Bearsampp-logo.svg"></a></p>

[![GitHub release](https://img.shields.io/github/release/bearsampp/module-ngrok.svg?style=flat-square)](https://github.com/bearsampp/module-ngrok/releases/latest)
![Total downloads](https://img.shields.io/github/downloads/bearsampp/module-ngrok/total.svg?style=flat-square)

This is a module of [Bearsampp project](https://github.com/bearsampp/bearsampp) involving ngrok.

## Documentation and downloads

https://bearsampp.com/module/ngrok

## Build (Gradle)

This project now uses Gradle (Groovy) for packaging releases, converted from the previous Ant `build.xml`.

Key behavior preserved and required by the original Ant build:
- The `ngrok.exe` binary is NOT embedded in this repository. It is downloaded during the build from the Bearsampp `modules-untouched` repository based on `modules/ngrok.properties`.
- The final archive includes the top-level version folder at the root (e.g., `ngrok3.19.1/...`).

Requirements:
- A local installation of Gradle (or use `gradlew` if you add a wrapper)
- Standard Windows tools; optional: `7z` in PATH for native 7z creation

How the Gradle build works:
1) Gradle determines the module version from the folder name under `bin/` that matches `bin/ngrok<version>` (e.g., `bin/ngrok3.19.1`). It mirrors Ant logic that relied on `bundle.path` to derive `bundle.version`.
2) It downloads `modules-untouched` properties file: `https://raw.githubusercontent.com/Bearsampp/modules-untouched/main/modules/ngrok.properties`.
3) It selects the download URL for `ngrok.exe` using the exact version key if available; otherwise it attempts `major.minor` then `major` keys. For legacy v2 builds (e.g., `2.2.8`), the build also tries a rich set of candidate URLs under common release tag patterns and filename variants (amd64/386 and `.zip`/`.7z`/`.exe`).
4) It downloads `ngrok.exe` into a staging area and prepares `build/tmp/prep/ngrok<version>` by copying:
   - the downloaded binary as `ngrok.exe` and
   - all files from `bin/ngrok<version>` (like `bearsampp.conf`).
5) It creates the release archive from `build/tmp/prep`, so that the folder `ngrok<version>` is preserved at the root of the archive.

Usage:
1. Ensure the versioned folder exists under `bin/` following the pattern `bin/ngrok<version>` (examples: `bin/ngrok3`, `bin/ngrok3.19.1`).
2. Adjust `build.properties` if needed:
   - `bundle.name = ngrok`
   - `bundle.release = YYYY.MM.DD`
   - `bundle.format = 7z` (default) or `zip`.
3. Run one of:

```
gradle release
# or
gradle bundleRelease
# or
gradle dist
```

Output:
- The artifact will be created under `build/dist/` with the name:

```
bearsampp-ngrok-<version>-<release>.<ext>
```

Notes:
- Override the Ngrok download URL directly (highest precedence) when necessary:
  - `-PngrokUrl=<DIRECT_URL>` or environment `NGROK_DOWNLOAD_URL`.
  This is useful if a specific mirror should be used for legacy versions.
- Override the source properties URL if needed via `-PuntouchedPropsUrl=<URL>` or environment variable `UNTOUCHED_PROPS_URL`.
- The downloader includes small retries and follows redirects with a standard User-Agent.
- If `7z` is not installed, Gradle falls back to a ZIP stream while keeping the `.7z` extension; for a true 7z container, install `7z` or set `bundle.format = zip`.

## Issues

Issues must be reported on [Bearsampp repository](https://github.com/bearsampp/bearsampp/issues).
