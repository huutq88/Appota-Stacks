<div align="center">

<img src="logo.png" width="80" height="80" alt="AppotaStacks Logo"/>

# AppotaStacks

**A lightweight, cross-platform Android emulator manager**  
Built on the AOSP Android Emulator stack · Qt 6 · C++20

[![Release](https://img.shields.io/github/v/release/huutq88/Appota-Stacks?style=flat-square&color=3d7eff)](https://github.com/huutq88/Appota-Stacks/releases)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Windows-informational?style=flat-square)](https://github.com/huutq88/Appota-Stacks/releases)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](LICENSE)
[![Qt](https://img.shields.io/badge/Qt-6.5%2B-41CD52?style=flat-square&logo=qt)](https://qt.io)

[📦 Download Latest Release](https://github.com/huutq88/Appota-Stacks/releases/latest) · [📋 Changelog](CHANGELOG.md) · [🐛 Report Bug](https://github.com/huutq88/Appota-Stacks/issues)

</div>

---

## ✨ Features

| Feature | Description |
|---|---|
| 🤖 **AVD Manager** | Create, list, start, stop, and delete Android Virtual Devices via GUI |
| 📱 **System Image Setup** | Browse and install system images with on-the-fly progress tracking |
| 🚀 **Emulator Launcher** | One-click launch with configurable RAM, CPU cores, and GPU mode |
| 📦 **APK Installer** | Drag-and-drop `.apk` installation to running emulators |
| 📜 **Logcat Viewer** | Real-time color-coded Android logs with level/tag filtering |
| 🗺️ **Key Mapping** | Map keyboard and mouse input to Android on-screen controls |
| 🔍 **SDK Auto-detect** | Automatically discovers Android SDK on macOS and Windows |
| 🎨 **Dark UI** | Modern glassmorphism design, built with Qt QML |

---

## 📥 Download

| Platform | Installer | Version |
|---|---|---|
| 🍎 macOS (Apple Silicon / Intel) | `appota_stacks_v1.0_b*.dmg` | v1.0.3 |
| 🪟 Windows 10/11 (x64) | `appota_stacks_v1.0_b*.exe` | v1.0.3 |

Download from the **[Releases](https://github.com/huutq88/Appota-Stacks/releases/latest)** page.

> **macOS**: Open the `.dmg`, drag **AppotaStacks** to Applications, then right-click → Open (first launch only, to bypass Gatekeeper).  
> **Windows**: Run the `.exe` installer. If SmartScreen warns, click "More info" → "Run anyway".

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Core Engine | C++20 — VM lifecycle, AVD management, ADB bridge |
| Graphics | Vulkan (primary) + OpenGL ES (fallback) |
| Desktop UI | Qt 6.5+ — C++ controllers + QML/QuickControls2 |
| Build | CMake 3.16+ · Python 3.10+ (`build.py`) |
| Android Layer | AOSP emulator + ADB + Kotlin companion APK |
| CI/CD | GitLab CI (Windows runner) + GitHub Releases |

---

## 🔧 Build from Source

### Prerequisites

| Dependency | Version | Notes |
|---|---|---|
| Qt 6 | 6.5+ | Qt Quick + QuickControls2 + Qt3D |
| CMake | 3.16+ | |
| Android SDK | API 33+ | platform-tools + emulator + cmdline-tools |
| Python | 3.10+ | For `build.py` |
| Xcode CLT *(macOS)* | Latest | `xcode-select --install` |
| MSVC 2022 *(Windows)* | Build Tools 2022 | [Download](https://visualstudio.microsoft.com/downloads/) |
| NSIS *(Windows, optional)* | 3.x | For `.exe` installer packaging |

### macOS

```bash
# Clone
git clone https://github.com/huutq88/Appota-Stacks.git
cd Appota-Stacks

# Debug build
python3 build.py --platform mac --qt-dir /path/to/Qt6/lib/cmake/Qt6

# Release build + package .dmg
python3 build.py --platform mac --qt-dir /path/to/Qt6/lib/cmake/Qt6 --release --package

# Run
open dist/AppotaStacks_v1.0_b*.dmg
```

### Windows

```powershell
# Clone
git clone https://github.com/huutq88/Appota-Stacks.git
cd Appota-Stacks

# Debug build (ensure Qt6 is in PATH or pass --qt-dir)
python build.py --platform win --qt-dir "C:\Qt\6.6.0\msvc2022_64\lib\cmake\Qt6"

# Release build + package .exe installer
python build.py --platform win --qt-dir "C:\Qt\6.6.0\msvc2022_64\lib\cmake\Qt6" --release --package

# Run
.\build\win_debug\bin\BlueStacksLite.exe
```

### CMake (manual)

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=/path/to/Qt6
cmake --build build --parallel
```

---

## 📁 Project Structure

```
Appota-Stacks/
├── core/                   # C++20 core library (libbluestacks_core)
│   ├── engine/             #   SDKDetector, EmulatorEngine, AVDManager, ADBBridge
│   ├── graphics/           #   VulkanRenderer, OpenGLRenderer, IRenderer
│   ├── hal/                #   InputMapper (keyboard/mouse → ADB events)
│   ├── image_manager/      #   ImageManager (system image downloads)
│   └── instance_manager/   #   InstanceManager (multi-instance support)
├── desktop/                # Qt 6 desktop application
│   ├── app/                #   main.cpp — bootstraps QML engine
│   ├── controllers/        #   C++ ↔ QML bindings
│   │   ├── EmulatorController  #   Launch / stop emulator
│   │   ├── AVDController       #   AVD CRUD + system images
│   │   ├── ADBController       #   ADB commands
│   │   ├── SettingsController  #   Persisted settings (QSettings)
│   │   ├── InstanceController  #   Multi-instance management
│   │   └── SetupController     #   First-run wizard, SDK setup
│   └── ui/                 #   QML UI
│       ├── pages/          #   Dashboard, AVDList, APKInstaller, Logcat, Settings, …
│       ├── components/     #   SideBar, AVDCard, ActionButton, StatusBadge, …
│       └── style/          #   Theme.qml (design tokens / dark theme)
├── android_guest/          # Android companion layer
│   └── kotlin_services/    #   Kotlin APK (boots with emulator, IPC bridge)
├── cmake/                  # CMake modules (FindAndroidSDK.cmake)
├── build.py                # Cross-platform build + package script
└── CMakeLists.txt          # Root CMake
```

---

## ⚙️ Configuration

Settings are persisted via `QSettings`:
- **macOS**: `~/Library/Preferences/com.appota.AppotaStacks.plist`
- **Windows**: `HKEY_CURRENT_USER\Software\Appota\AppotaStacks`

| Setting | Default | Description |
|---|---|---|
| `sdkPath` | Auto-detect | Android SDK root path |
| `defaultRam` | `2048` MB | VM RAM |
| `defaultCores` | `4` | VM CPU core count |
| `gpuMode` | `host` | `host \| swiftshader_indirect \| angle_indirect \| off` |

---

## 🚀 CI/CD

| Platform | Pipeline | Trigger |
|---|---|---|
| **Windows** | GitLab CI (`build-windows` → `publish-github`) | Push tag `v*.*.*` or manual |
| **macOS** | Built locally, uploaded manually to GitHub Releases | Manual |

The Windows CI automatically uploads the `.exe` to the corresponding GitHub Release.  
If a file with the same name already exists, it is deleted and re-uploaded.

---

## 📋 Changelog

See [CHANGELOG.md](CHANGELOG.md) for the full history.

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit with conventional commits: `git commit -m "feat: add my feature"`
4. Push and open a Pull Request

---

## 📄 License

MIT — See [LICENSE](LICENSE)

