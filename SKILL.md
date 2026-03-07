---
name: macos-disk-cleanup
description: Analyze and clean up macOS disk space. Finds large caches, simulators, build artifacts, and other space hogs. Use when a user wants to free up storage, check disk usage, or clean up their Mac.
---

# macOS Disk Cleanup

Analyze disk usage and safely clean up space on macOS.

## When to Use

- User says "free up space", "disk full", "storage cleanup", "clean up Mac"
- User is running low on disk space
- User wants to know what's taking up space

## Step 1: Assess Disk Usage

```bash
df -h /
```

Then scan the largest directories in the home folder:

```bash
du -sh ~/Library/Developer ~/Library/Caches ~/Library/Application\ Support ~/Downloads ~/.Trash ~/Desktop ~/Documents 2>/dev/null | sort -rh
```

## Step 2: Identify Cleanup Targets

Check each category and report sizes. Present a summary table to the user before deleting anything.

### iOS Simulators (often 10-40GB)

```bash
du -sh ~/Library/Developer/CoreSimulator 2>/dev/null
xcrun simctl list runtimes 2>/dev/null
```

- Delete unavailable simulators: `xcrun simctl delete unavailable`
- Delete old runtimes: `xcrun simctl runtime delete <runtime-id>`
- Show which runtimes have no devices before suggesting deletion

### Xcode Build Artifacts (often 5-20GB)

```bash
du -sh ~/Library/Developer/Xcode/DerivedData ~/Library/Developer/Xcode/Archives ~/Library/Developer/Xcode/iOS\ DeviceSupport 2>/dev/null | sort -rh
```

- DerivedData: Always safe to delete (rebuilds automatically)
- Archives: Safe if user doesn't need old app builds
- iOS DeviceSupport: Safe for old iOS versions user no longer tests on

### Package Manager Caches

```bash
du -sh ~/Library/Caches/Homebrew ~/.npm ~/Library/pnpm ~/Library/Caches/pip ~/Library/Caches/go-build ~/Library/Caches/CocoaPods 2>/dev/null | sort -rh
```

Clean commands:
- npm: `npm cache clean --force`
- Homebrew: `brew cleanup --prune=all`
- pip: `pip cache purge`
- pnpm: `pnpm store prune`
- Go: `go clean -cache`

### Application Caches (~/Library/Caches)

```bash
du -sh ~/Library/Caches/*/ 2>/dev/null | sort -rh | head -20
```

Safe to delete:
- `go-build/` - Go build cache
- `*.ShipIt/` - App update caches (Cursor, Claude, etc.)
- `vscode-cpptools/` - VS Code C++ intellisense cache
- `JetBrains/` - JetBrains IDE cache
- `Google/` - Chrome cache
- `node-gyp/` - Node native module build cache
- `gopls/` - Go language server cache

Do NOT delete without asking:
- `ms-playwright/` - Playwright browsers (takes time to reinstall)
- `Unity/` - Unity asset cache (may be large and slow to rebuild)

### Docker

```bash
docker system df 2>/dev/null
```

- `docker system prune -f` for unused containers/networks/images
- `docker builder prune -f` for build cache

### Trash

```bash
du -sh ~/.Trash 2>/dev/null
```

- Empty with: `rm -rf ~/.Trash/*`

### Homebrew

```bash
brew cleanup --dry-run 2>/dev/null | tail -5
```

- Clean with: `brew cleanup --prune=all`

## Step 3: Present and Confirm

ALWAYS present a summary table before cleaning:

```
| Target | Size | Risk |
|--------|------|------|
| Xcode DerivedData | 6.6G | Safe (rebuilds automatically) |
| iOS Simulators (old) | 9G | Safe (re-download if needed) |
| npm cache | 1.3G | Safe |
| ... | ... | ... |
```

Ask the user which items to clean. Never delete without confirmation.

## Step 4: Clean and Report

After cleaning, show before/after disk space:

```bash
df -h /
```

Report total space recovered.

## Important Rules

1. NEVER delete user data (Documents, Desktop, Downloads) without explicit confirmation
2. NEVER delete application data in ~/Library/Application Support without asking
3. ALWAYS show sizes before deleting
4. ALWAYS ask for confirmation before bulk deletions
5. Prefer targeted cleanup over blanket `rm -rf ~/Library/Caches/*`
6. Check if Xcode/simulators are actively in use before suggesting deletion
