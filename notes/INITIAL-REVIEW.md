# CLVR — Initial Code Review

*Review by Lēsa, February 16, 2026*

## What CLVR Is Today

A macOS menu bar utility that watches Desktop/Documents folders for file duplications (Cmd+D). When it detects a file with "copy" in the name, it renames it from `filename copy.ext` to `filename--YYYY-MM-DD--HH-MM-SS.ext`.

- **On the Mac App Store** (4 stars)
- **Open source** (MIT license, with copyright on branding/binaries)
- **Built with AI** — Claude Sonnet 3.5, ChatGPT 4o, Claude Engineer CLI, Cursor, Xcode
- **Contains extensive AI development docs** in `ai-insights/`

## Codebase Summary

### Architecture
- **2 Swift files**: `CLVRApp.swift` (~30 lines, entry point) + `AppDelegate.swift` (~900 lines, everything else)
- **All AppKit** — no SwiftUI
- **Manual frame-based layout** — hardcoded pixel positions for every UI element
- **FSEvents** for filesystem watching
- **UserDefaults** for settings persistence
- **Security-scoped bookmarks** for Documents folder access

### Core Flow
1. App launches → sets up menu bar status item
2. `FileSystemWatcher` monitors Desktop + Documents via FSEvents
3. On file event → `shouldRenameFile()` checks if name ends with " copy" or " copy N"
4. If yes → monitors for stability (3 checks, 0.5s apart to wait for copy to finish)
5. Renames file with timestamp format
6. Animates menu bar icon as visual feedback

### Settings
- Show in Dock (toggle)
- Show in Menu Bar (toggle)
- Two naming formats:
  - `name--yyyy-MM-dd--HH-mm-ss.ext` (default)
  - `name-copy--yyyy-MM-dd--HH-mm-ss.ext`

### What's Good
- **Core mechanic is solid** — FSEvents + stability checking before rename works well
- **Clean separation** of concerns in the rename flow
- **Extensive AI development documentation** — every decision documented
- **App Store published** — already through Apple review
- **Comprehensive logging** — file-based log system

### What Needs Work for the Fork
- **Monolithic AppDelegate** — 900 lines doing everything (UI, logic, file watching, settings)
- **All manual layout** — `NSRect(x: 20, y: yOffset, width: 520, height: 20)` everywhere
- **No data persistence beyond UserDefaults** — no database for tracking operations
- **Limited folder scope** — only Desktop + Documents
- **No Finder integration** — no context menu, no Quick Look
- **Settings window recreated on every open** — not a persistent view

## Expansion Ideas Discussed

### Selected Direction: "Version Instead of Duplicate"
- Intercept duplication → offer to create a version (git commit) instead
- Delete the duplicate → keep desktop clean
- Right-click → "Time Warp" → timeline UI to browse/restore versions
- Git as invisible backend, optional GitHub sync
- Agent-queryable version history

### Other Ideas (Backlog)
1. **File Memory Layer** — every file operation becomes searchable memory
2. **Smart Naming** — context-aware names instead of just timestamps
3. **File Activity Stream** — all file ops, not just duplicates
4. **Folder Rules Engine** — simple Hazel alternative
5. **Screenshot Intelligence** — auto-organize by content/project
6. **Agent Attribution** — tag files with what/who created them
7. **Team File Awareness** — distributed file activity for enterprise
8. **Non-Code Version Browsing** — git for design files, docs, spreadsheets

## Recommended Build Order

1. **SwiftUI rewrite** of existing functionality (same features, modern code)
2. **Git backend** — local repo for version storage
3. **Version-on-duplicate flow** — the notification + commit + delete-copy
4. **Finder Extension** — right-click "Open in Time Warp"
5. **Time Warp UI** — timeline browser
6. **GitHub sync** — optional cloud backup
7. **Agent bridge** — expose version history to OpenClaw

---

*Fork: wipcomputer/CLVR*
*Original: parkertoddbrooks/CLVR*
