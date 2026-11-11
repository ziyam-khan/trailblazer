<h1 align="center">trailblazer</h1>
<p align="center">A fast desktop file explorer: a Rust core that indexes every drive, behind a Tauri and React window.</p>

<p align="center">
  <img alt="Rust" src="https://img.shields.io/badge/core-Rust-b7410e">
  <img alt="Tauri" src="https://img.shields.io/badge/shell-Tauri%201-24c8db">
  <img alt="React" src="https://img.shields.io/badge/ui-React%2018-61dafb">
  <img alt="Linux" src="https://img.shields.io/badge/platform-Linux-333">
</p>

## What it does

- **Indexes each volume once, then keeps up.** On first launch the Rust side walks every mounted drive into an in-memory cache, and a file-system watcher (`notify`) applies creates, deletes and renames as they happen, so the cache never needs a rescan. The cache is saved to disk compressed (bencode + zstd) at an interval, so the next launch starts warm.
- **Searches as you type.** Fuzzy matching (the skim algorithm, `fuzzy-matcher`) scores every cached path in parallel (`rayon`), with filters for files, folders and an extension.
- **Does the everyday file work.** Browse drives and folders, open files with their default app, create, rename and delete, from a right-click menu.

## Stack

| Layer | Tools |
|---|---|
| Core | Rust, Tokio, `walkdir`, `notify`, `sysinfo`, `fuzzy-matcher`, `rayon`, `serde_bencode`, `zstd` |
| Shell | Tauri 1 (IPC commands in `src-tauri/src`) |
| Interface | React 18, TypeScript, Redux Toolkit, Tailwind CSS, Vite |

## Run it

You need Rust, Node.js with Yarn, and the [Tauri prerequisites](https://tauri.app/v1/guides/getting-started/prerequisites) for your system.

```sh
cargo install tauri-cli   # once
yarn                      # install the interface's dependencies
cargo tauri dev           # run in development
cargo tauri build         # build a release bundle
```

## Layout

| Path | What is there |
|---|---|
| `src-tauri/src/filesystem/` | Volumes, the explorer commands and the watched cache |
| `src-tauri/src/search.rs` | Fuzzy search over the cache, with its filters |
| `src/` | The React interface: volumes, folder view, search bar, context menus |

---

By [Ziyam Khan](https://ziyamkhan.com) · [GitHub](https://github.com/ziyam-khan)
