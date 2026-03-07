# macos-disk-cleanup

A Claude Code skill for analyzing and cleaning up macOS disk space.

## What it does

- Scans for large caches, build artifacts, simulators, and other space hogs
- Presents a clear summary with sizes and risk levels
- Safely cleans selected targets with user confirmation

## Cleanup targets

- **iOS Simulators** - old runtimes and unavailable devices (10-40GB)
- **Xcode** - DerivedData, Archives, DeviceSupport (5-20GB)
- **Package managers** - npm, pnpm, pip, Homebrew, Go, CocoaPods
- **App caches** - Chrome, JetBrains, VS Code, Cursor, etc.
- **Docker** - unused containers, images, build cache
- **Trash** - forgotten items in trash

## Install

```bash
npx skills add JadenChoi2k/macos-disk-cleanup
```
