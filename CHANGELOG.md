# Changelog

All notable changes to **AppotaStacks** are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).  
Versioning follows [Semantic Versioning](https://semver.org/).

---

## [1.0.3] — 2026-03-18

### Added
- Version label displayed beneath **APPOTA LITE** branding in sidebar
- `CHANGELOG.md` — this file

### Fixed
- **Windows: AVD list empty after creating a device**
  - `homeDir()` now uses `USERPROFILE` on Windows (`HOME` env var is not set by default on Windows)
  - `listAVDs()` replaced Unix `find *.ini` shell command with `std::filesystem::directory_iterator` (portable C++17)
  - `listSystemImages()` replaced Unix `find -mindepth 3 -maxdepth 3 -type d` with `std::filesystem` recursive iteration
  - `iniValue()` now strips trailing `\r` from CRLF line endings in Windows `.ini` files and normalises `\` → `/` in path values
- **Windows: Terminal window flashing every 3 seconds while emulator boots**
  - `waitForBoot()` called `_popen` every 3 seconds → a visible `cmd.exe` window flashed for each `adb` poll
  - `stopEmulator()` used `std::system()` → caused a flash on emulator stop
  - `startEmulator()` used `_popen` to launch the emulator → caused an extra flash on start
  - Fix: replaced all `_popen`/`std::system` on Windows with `CreateProcess` + `CREATE_NO_WINDOW` + anonymous pipes
- **About section in Settings showed wrong data**
  - App name corrected to `AppotaStacks`
  - Version now reads from `QCoreApplication::applicationVersion()` (was hardcoded `1.0.0`)
  - GitHub link corrected to `https://github.com/huutq88/Appota-Stacks`
- **CI: re-running same tag would fail with HTTP 422**
  - `publish-github` job now deletes the existing GitHub Release asset before re-uploading
- **CI: `publish-github` could not be triggered manually**
  - Added `CI_PIPELINE_SOURCE == "web"` rule with `RELEASE_TAG` variable override for manual pipeline runs

### Changed
- README rewritten to be more professional and comprehensive
- `publish-github` CI job uses `RELEASE_TAG` variable to support manual releases independent of git tags

---

## [1.0.2] — 2026-03-15

### Added
- Windows installer packaging via NSIS (`appota_stacks_v1.0_b*.exe`)
- GitLab CI pipeline for Windows builds (`build-windows` job)
- `publish-github` CI job — uploads `.exe` artifact to GitHub Releases automatically
- Automatic `.exe` discovery using `rglob("*.exe")` with full build-tree diagnostics

### Fixed
- CMake `--config Release` flag now passed when building with MSVC Visual Studio generator (previously ignored `CMAKE_BUILD_TYPE`)
- `CL.exe` (MSVC compiler) exit code 4 — resolved by passing correct Release config flag
- Artifact upload path mismatch in CI (`dist/appota_stacks_*.exe` vs `build/win_release/bin/*.exe`)

### Changed
- `build-mac` job removed from GitLab CI (no macOS runner available; macOS builds done locally)
- `publish-github` ported from Alpine/curl to PowerShell (`Invoke-RestMethod`) for Windows runner compatibility

---

## [1.0.1] — 2026-03-10

### Added
- Multi-instance support (`InstanceManager`, `InstanceController`)
- Key mapping page (`KeyMappingPage.qml`)
- `SetupWizardPage` — guided first-run flow for Android SDK installation
- `GuestInstaller` — auto-installs Kotlin companion APK into running emulator
- Logcat viewer with real-time filtering by log level and tag

### Fixed
- OpenGL header includes for Windows (`windows.h` + `GL/gl.h` ordering)
- `popen`/`pclose` replaced with `_popen`/`_pclose` on Windows
- `std::filesystem::path` string concatenation incompatibility on MSVC

---

## [1.0.0] — 2026-03-05

### Added
- Initial release
- AVD Manager — create, list, start, stop, delete Android Virtual Devices
- APK Installer — drag-and-drop `.apk` installation via ADB
- Settings page — SDK path, RAM, CPU cores, GPU mode
- Android SDK auto-detection on macOS and Windows
- Vulkan renderer with OpenGL fallback
- Dark glassmorphism UI (Qt 6 QML)
- Android companion Kotlin APK (`kotlin_services`)
- `build.py` — unified cross-platform build script
