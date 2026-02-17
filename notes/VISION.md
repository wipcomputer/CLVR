# CLVR — Time Warp Vision

*From file duplication to file memory.*

---

## The Insight

People don't want duplicates. They want versions.

When someone hits Cmd+D, they're not trying to create clutter. They're trying to preserve a state before making changes. "Let me save this before I mess it up." But the result is `budget copy.xlsx`, `budget copy 2.xlsx`, `budget FINAL.xlsx`, `budget FINAL v2 REAL.xlsx`.

The real need is **version history**, not more copies.

## What CLVR Becomes

CLVR intercepts the duplication impulse and converts it into proper versioning:

1. User hits **Cmd+D** on a file → macOS creates "file copy"
2. CLVR catches this (like it already does) and shows a notification: **"Create a version instead?"**
3. User taps **Yes** →
   - CLVR commits the current state of the original file to a Git repo (invisible to the user)
   - CLVR **deletes the duplicate** — no more "file copy" cluttering the folder
4. Result: **One file. Many versions. Clean desktop.**

To access old versions:
- **Right-click** the file → **"Open in Time Warp"**
- A beautiful timeline UI appears (inspired by Apple's Time Machine, but for a single file)
- **Scroll back and forth** through every version you chose to save
- Click to **restore** any version

## What This Replaces

| Before CLVR | After CLVR |
|---|---|
| `pitch-deck copy.pptx` | One file, versioned |
| `pitch-deck copy 2.pptx` | Right-click → Time Warp |
| `pitch-deck FINAL.pptx` | Scroll through timeline |
| `pitch-deck FINAL v2 REAL.pptx` | Click to restore |
| 4 files cluttering Desktop | 1 file, full history |

## The User Experience

### Creating a Version
```
User: Cmd+D on "proposal.docx"

CLVR notification: "Create a version instead?"
  [Yes]  [No, just duplicate]

User taps Yes →
  ✓ Version saved (Feb 16, 2026 6:45 PM)
  ✓ Duplicate removed
  ✓ Desktop stays clean
```

### Browsing History
```
User: Right-click "proposal.docx" → "Open in Time Warp"

Timeline UI appears:
  ←  Feb 12  |  Feb 14  |  Feb 16  →
     v1          v2         v3 (current)

User clicks Feb 14 version → Preview opens
User clicks "Restore" → File reverts to that version
  (current version auto-saved before restore)
```

### Smart Learning
Over time, CLVR learns your preferences:
- You always version `.sketch` files? Stop asking, just do it.
- You never version `.tmp` files? Stop asking about those.
- Pattern matching: "If file is in ~/Projects/, auto-version. If in ~/Downloads/, don't ask."

## Technical Architecture

### Storage Backend
- **Git** as the versioning engine — invisible to the user
- Local `.clvr/` repo (or `~/.clvr/repos/`) with git history
- Optional **GitHub sync** for cloud backup (private repos, free)
- User never sees git, never types a command

### macOS Integration
- **Finder Extension** → right-click context menu "Open in Time Warp"
- **Notification Center** → version prompt on duplicate
- **Spotlight integration** → search across versions
- **Quick Look** → preview old versions without restoring

### Time Warp UI
- SwiftUI-based timeline browser
- Single-file focused (not whole-system like Time Machine)
- Side-by-side diff view for text files
- Visual preview for images, PDFs
- Calendar view for version density

## Why Git?

- Free, proven, distributed
- Works offline
- Handles binary files (with LFS for large ones)
- GitHub gives free private repos for cloud sync
- Diffs are built-in for text formats
- The agent can query git log programmatically

## The Agent Connection

This is where CLVR becomes a WIP product:

- **"Hey Lēsa, what did the proposal look like last Tuesday?"** → She checks the git log, pulls the commit, shows you
- **"Restore the version of budget.xlsx from before the board meeting"** → She finds it by date/context, restores it
- **"What files did I version this week?"** → Activity summary from git log
- In the **enterprise product**, file versioning becomes part of the corporate memory layer

## Product Positioning

**Current CLVR:** "Your duplicated files get timestamps."
**Time Warp CLVR:** "Your files remember themselves."

> Duplicate any file. We'll ask if you want to remember it.
> Say yes. The duplicate disappears. The memory stays.
> Right-click any file. Scroll through time. Restore anything.

## macOS 26 Modernization

The current codebase is AppKit with manual frame-based layout (~900 lines of hardcoded pixel positions). The fork should:

1. **SwiftUI rewrite** — native macOS 26 look, half the code
2. **Finder Extension** — right-click integration for Time Warp
3. **Modern notification style** — inline actions in notification banner
4. **Settings via SwiftUI** — replace the manual NSRect settings window

## Open Questions

- **Name:** Keep "CLVR" or rename for the WIP version? "Time Warp" is strong for the feature, but the app needs a name too.
- **Scope:** Start with just the version-on-duplicate flow? Or build Time Warp UI simultaneously?
- **Backend:** Git-only, or abstract the storage layer for future alternatives?
- **Pricing:** Free tier (local only) + paid tier (GitHub sync + agent integration)?

---

*Notes from a conversation between Parker and Lēsa, February 16, 2026.*
*Original repo: github.com/parkertoddbrooks/CLVR*
*Fork: github.com/wipcomputer/CLVR*
