# Harbor · Workspace Dock

> [!IMPORTANT]
> **This repository has moved.** The official Harbor repository is now [YiChenLu0119-prog/harbor-workspace-dock](https://github.com/YiChenLu0119-prog/harbor-workspace-dock).
>
> All future development, updates, bug reports, and feature requests belong in the [new repository](https://github.com/YiChenLu0119-prog/harbor-workspace-dock) and its [Issues](https://github.com/YiChenLu0119-prog/harbor-workspace-dock/issues). This repository is retained as a historical copy and will no longer receive updates.
>
> **已迁移：**后续更新与反馈统一在[新仓库](https://github.com/YiChenLu0119-prog/harbor-workspace-dock)，本仓库仅保留历史版本。

English | [简体中文](README.zh-CN.md)

**A small home for your ChatGPT work on macOS.**

Harbor keeps your conversation shortcuts together by project. Press **⌃⌥Space**, find an entry, and continue working in the native app. Built with SwiftUI and AppKit, with local storage and no third-party dependencies.

## Features

- **One shortcut away.** A menu-bar app and a global keyboard shortcut open a native floating panel.
- **Organize your work.** Projects, search, favorites, editable entries, and full-row navigation targets.
- **Move, sort, and undo.** Delete entries with undo, move them between projects, or drag to reorder within a project.
- **Make it yours.** English and Simplified Chinese, system/light/dark appearance, accent colors, list density, and a preferred starting view.
- **Ready at login.** Optional macOS login launch, with the actual system registration status shown in Settings.
- **Local by design.** Project names, links, and confirmation records live on your Mac. Harbor does not read conversations, cookies, or account credentials.

**Native opening currently supports local Codex thread links only.** Cloud ChatGPT links can be organized and saved, but native opening is not yet supported. See [link support](#link-support) below.

## Get started

Requires **macOS 13+**, full Xcode, and Swift 5.9 or later.

```sh
bash scripts/test.sh
bash scripts/build.sh
open dist/Harbor.app
```

Install `Harbor.app` in `/Applications` or `~/Applications` before enabling **Launch at login**. This is a local development build with an ad-hoc signature; Developer ID signing and Apple notarization are not available yet. See [Build and run](BUILDING.md).

1. Create a project, then add an entry with a title and the original link copied from the native app.
2. For supported native links, choose **Verify**, inspect the destination in the native app, and return to Harbor to confirm the exact conversation.
3. Press **⌃⌥Space**, click Search or press **⌘F**, then type. Select with **↑/↓** and press **Return** to open or verify.
4. Use **⌘N** to add an entry, **⌘F** to focus search, and **⌘,** to open Settings.

New installations start in English. Switch anytime in **Settings → Language**. Existing preferences from earlier versions retain Simplified Chinese. Your own project names and entry titles stay as written. Opening the panel or switching projects leaves the search field inactive; click it or press **⌘F** to start typing.

## What's new in v0.3

| Action | How it works |
| --- | --- |
| Delete and undo | Open an entry's **…** menu and choose **Delete entry**. Use the banner's **Undo** action, the menu-bar command, or **⌘Z** to restore it. This removes only the Harbor bookmark; the original conversation stays in ChatGPT. |
| Move to a project | Choose **Move to…**, or drag the entry onto a project in the sidebar. Its link, favorite state, and verification record stay intact. |
| Reorder | Drag above or below another entry in the same project, or choose **Move up / Move down**. Clear search and leave Favorites before reordering. |
| Larger click targets | Click anywhere in a sidebar row, including its icon, count, and blank space. Click the body of an entry row to select it; its action and menu remain separate. |
| Change language | Choose **English** or **简体中文** in Settings. The interface changes immediately and remembers your choice. |

Undo covers the latest 50 successful library changes during the current run, including edits and favorites. It clears when the app quits. Changes and ordering are saved immediately; text-field undo takes priority while editing. Preferences have separate storage and do not consume library undo history.

## Link support

| Link | Behavior |
| --- | --- |
| `codex://threads/<local-thread UUID>` | Tests opening in the designated native app; the exact conversation must be confirmed by the user. |
| Cloud URLs such as `https://chatgpt.com/c/…` | Saved for organization; native opening is currently unavailable. |
| `chatgpt-conversation://…` references | Saved for organization; native opening is currently unavailable. |
| Other URLs or nonstandard native links with extra parameters | Not executed; Harbor explains why. |

The [official deep-link reference](https://learn.chatgpt.com/docs/reference/commands#deep-links) documents the local-thread route. A registered URL handler or an accepted system request does not prove that the correct conversation opened. Harbor never converts a cloud conversation ID into a local thread ID and never silently falls back to a browser. A changed URL or native app version requires confirmation again.

## Design and architecture

- [Architecture and storage](docs/ARCHITECTURE.md)
- [Entry management and interaction design](docs/entry-management.md)
- [UI research and decisions](docs/design-research.md)
- [Roadmap](docs/ROADMAP.md)

`HarborCore` owns models, storage, editing, undo, and link policy. `HarborApp` provides SwiftUI, the menu bar, the floating panel, keyboard handling, settings, and native opening. Unreadable or unsupported-version data is protected from overwrite.

## Privacy

Data stays in `~/Library/Application Support/HarborWorkspaceDock/`. This repository contains source, fictional test fixtures, and public documentation only. Personal libraries, conversation identifiers, confirmation records, and private development history are excluded. Please keep private chat links out of public issues and screenshots.
