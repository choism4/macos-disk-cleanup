# macos-disk-cleanup

A Claude Code skill for analyzing and cleaning up macOS disk space. Built for everyone -- not just developers.

## What it does

- Scans your Mac for space hogs and presents a clear summary with sizes and safety levels
- Supports three modes: **Scan Only**, **Cleanup**, and **Targeted** ("I need 10GB free")
- Always asks before deleting anything
- Checks if apps are running before touching their caches
- Gracefully skips tools that aren't installed (Xcode, Docker, Homebrew, etc.)

## Cleanup targets (18 categories)

| Category | Typical Size | For |
|----------|-------------|-----|
| Trash | 0-50GB | Everyone |
| Downloads (old DMGs, ZIPs) | 2-30GB | Everyone |
| Photos, iMovie, GarageBand | 5-200GB | Everyone |
| Mail attachments | 1-10GB | Everyone |
| iMessage attachments | 0.5-15GB | Everyone |
| Music, Podcasts, Apple Music | 1-30GB | Everyone |
| Browser caches | 1-10GB | Everyone |
| Cloud storage caches | 1-20GB | Everyone |
| iOS/iPadOS backups | 5-50GB | Everyone |
| Application caches | 2-20GB | Everyone |
| Large apps | 1-15GB each | Everyone |
| Old macOS installers | 5-13GB each | Everyone |
| Uninstalled app leftovers | 0.5-5GB | Everyone |
| Screen recordings | 0.5-10GB | Everyone |
| System data & Time Machine | 5-30GB | Everyone |
| Fonts & printer drivers | 0.5-5GB | Everyone |
| Xcode, simulators, DerivedData | 10-60GB | Developers |
| npm, pip, Docker, Homebrew, Cargo, etc. | 5-30GB | Developers |

## Install

```bash
npx skills add choism4/macos-disk-cleanup
```

## Safety

- Never deletes without user confirmation
- Checks if apps are running before clearing caches
- Guards developer tool commands with existence checks
- Warns about SIP-protected paths and cloud-synced folders
- Shows before/after disk usage for every session
