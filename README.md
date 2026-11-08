# A4P 파일 뷰어 (a4p-file-viewer)

herdr 칸 옆에 여는 **읽기 전용 파일 뷰어**입니다. 파일을 고치거나 지우지 않습니다.
[smarzban/herdr-file-viewer](https://github.com/smarzban/herdr-file-viewer) 1.18.0(MIT)을 목회자 수강생용으로 고친 판입니다.

## 원본에서 바꾼 것

- **`→` 로 열고 `←` 로 돌아오기**: 목록에서 파일 위의 `→` 는 `Enter` 처럼 파일을 크게 엽니다. 파일을 보다가 `←` 를 누르면 목록으로 돌아옵니다. 긴 줄을 `→` 로 옆으로 밀어 둔 상태라면 `←` 가 먼저 왼쪽으로 돌아가고, 맨 왼쪽에서 한 번 더 누르면 목록으로 갑니다. 원본에서는 파일을 연 뒤 `←` 가 아무 일도 하지 않았습니다.
- **`Esc`(또는 `q`)는 한 단계씩 뒤로**: 파일 → 목록 → 뷰어 닫기. 원본은 파일 칸에서 누르면 바로 닫혔습니다.
- **아래 테두리에 한국어 키 안내**: 목록에서는 `→ 열기 · Esc 닫기 · ? 도움말`, 파일을 볼 때는 `← 목록으로 · ? 도움말`. 칸이 좁으면 뒤쪽(`? 도움말`)을 뺍니다. 좁은 칸에서 목록만 보일 때는 목록 아래 테두리에 나옵니다. 원본은 영어 `? help` 하나였습니다.
- 플러그인 이름을 `a4p-file-viewer`(버전 `1.18.0-a4p.1`)로 바꿔서 원본과 같이 깔려도 겹치지 않습니다. 버전 꼬리표 덕분에 설치 단계가 원본 릴리스 실행 파일을 잘못 내려받지 않고 소스에서 빌드합니다.

## 설치 (이 맥에서 빌드)

필요한 것: macOS, herdr 0.7.5 이상, Rust(cargo) 1.96 이상.

```sh
cargo build --release
herdr plugin link "$PWD"
```

단축키(`~/.config/herdr/config.toml`) — 원본의 `herdr-file-viewer.` 를 `a4p-file-viewer.` 로 바꿉니다.

```toml
[[keys.command]]
key = "prefix+f"
type = "plugin_action"
command = "a4p-file-viewer.open-file-viewer"
description = "파일 뷰어 (오른쪽 칸)"
```

설정 파일은 `herdr plugin config-dir a4p-file-viewer` 폴더의 `config.toml` 입니다(항목 설명: `config.example.toml`). 원본 업데이트 알림은 원본 기준이라 `update_check = false` 를 권합니다.

## 원본 업데이트 반영

`upstream` 에서 새 태그를 받아 `a4p` 브랜치를 그 위로 옮긴 뒤 `cargo test --release` 가 모두 통과하는지 봅니다. 고친 곳은 코드에서 `A4P:` 로 찾을 수 있습니다.

---

# herdr-file-viewer

[![CI](https://github.com/smarzban/herdr-file-viewer/actions/workflows/ci.yml/badge.svg)](https://github.com/smarzban/herdr-file-viewer/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Rust 1.96+](https://img.shields.io/badge/rust-1.96%2B-orange.svg)
![herdr 0.7.5+](https://img.shields.io/badge/herdr-0.7.5%2B-8a2be2)
![platforms: linux • macOS • Windows (preview)](https://img.shields.io/badge/platforms-linux%20%E2%80%A2%20macOS%20%E2%80%A2%20Windows%20(preview)-informational)

**A git-aware, read-only file viewer in a herdr pane.** Tree on the left. On the right, the view
that file deserves: a **diff** if it changed, **rendered markdown**, or **highlighted code**.
Agents can drop you on a file or a line. You pin one file, mark a range, and paste those notes
back into the chat. It never touches your files.

> [!TIP]
> Get **[tsk](https://github.com/smarzban/tsk)** to keep your work and your agents' work on one board. TUI for you, CLI for them.

![herdr-file-viewer open in a herdr split beside your work: the directory tree on the left, syntax-highlighted content on the right](assets/File-viewer.png)

*The right view per file, here a markdown file rendered (headings, inline code, tables) in your terminal's theme:*

![herdr-file-viewer rendering a markdown file: colored headings and styled inline code on the right, the git-status tree on the left](assets/Markdown-view.png)

*The same tree and content can fill the terminal:*

![herdr-file-viewer running full-screen](assets/File-Viewer-FS.png)

*Pin a file with `p` and keep browsing: tree, the file you are on, and a frozen `Pinned: [main]` beside it:*

![herdr-file-viewer with a pinned preview: tree on the left, the active file in the middle, a frozen pin of another file on the right](assets/Pinned-preview.png)

## Why you'd want it

- **The right view, automatically.** A changed file opens as a diff. A README renders. Code is
  highlighted. No `cat`, no mode switch, no commands. Press `v` only when you want something else.
- **Find the path or the text.** `f` fuzzy-finds a file name; `s` searches project contents and
  shows each matching `path:line` with an excerpt. Press `Enter` to open the source at that line.
- **Git in the tree.** `M`/`A`/`D`/`?` on every row, a changed-only filter (`c`), jump next/prev
  changed file (`]`/`[`), flip the baseline between your branch and `HEAD` (`b`). Not a separate
  git client.
- **Pin one file, keep browsing.** `p` freezes it on the right. Switch worktree (`W`) and compare
  it with another checkout, or pin the old version and walk the new one.
- **Agents show you the spot. You send notes back.** Teach them the [bundled skill](skills/herdr-file-viewer/SKILL.md)
  and "open `src/app.rs:42` in Files" lands you there. Mark a file or a range (`a`), copy the
  notes (`A` then `y`), paste them into the chat.
- **Edit in *your* editor.** `e` suspends the viewer and opens the file in neovim, vim, micro, or
  whatever you set as `editor` (else `$EDITOR`). You change the file there; the viewer never writes
  it, and comes back when you quit the editor.
- **Beside your work, safe on anything.** One keypress in a herdr split (or its own tab). Read-only,
  hardened for an agent's worktree or a fresh clone. Delegates rendering to `glow` / `delta` / `bat`.
  See [SECURITY.md](SECURITY.md).

## Highlights

The [full key and mouse reference](docs/keys.md) lists every binding. The [usage
guide](docs/usage.md) explains each feature. These are the main keys:

| Key | Does |
| --- | --- |
| `f` | Fuzzy-find any file in the tree |
| `s` | Search project contents; open a matching `path:line` with `Enter` |
| `p` | Pin the current file and keep browsing (compare across worktrees with `W`) |
| `a` / `A` | Annotate a file or range; copy the notes out for an agent |
| `v` | Cycle the view (diff ⇄ rendered ⇄ syntax) |
| `]` / `[` | Jump to the next / previous changed file |
| `b` | Flip the diff baseline: your branch's merge-base ⇄ `HEAD` |
| `W` | Switch to another git worktree, in place |
| `L` | Copy a `path:line` reference (or the selected lines) |
| `Z` | Full-screen the current file |
| `e` / `O` / `R` | Hand off: editor / OS default app / file manager |
| `?` | Help overlay: What's New, keys, settings, about |

## Quick start

```bash
# 1. Install the plugin (downloads a prebuilt binary for released versions; otherwise builds from source):
herdr plugin install smarzban/herdr-file-viewer

# 2. (recommended) install the renderers, so markdown / diffs / code are styled, not plain text:
brew install glow git-delta bat     # macOS, or use your package manager
#   Linux / cross-platform: run scripts/install-renderers.sh from the plugin dir (`herdr plugin list`)
```

Then **bind a key** in your herdr config (`~/.config/herdr/config.toml`) so one press summons it:

```toml
[[keys.command]]
key = "prefix+f"
type = "plugin_action"
command = "herdr-file-viewer.open-file-viewer"
description = "open file viewer in split"

[[keys.command]]
key = "prefix+shift+f"
type = "plugin_action"
command = "herdr-file-viewer.open-file-viewer-tab"
description = "open file viewer in tab"

[[keys.command]]
key = "prefix+d"
type = "plugin_action"
command = "herdr-file-viewer.open-file-viewer-at"
description = "open file viewer at… (split)"   # asks for a directory in a popup

[[keys.command]]
key = "prefix+alt+d"
type = "plugin_action"
command = "herdr-file-viewer.open-file-viewer-at-tab"
description = "open file viewer at… (tab)"     # same popup, opens a new tab
```

Run `herdr server reload-config`, then press your key. The plugin includes the split-pane viewer and
its open actions, which register during installation. You only need to add the keybinding.

Once the viewer has focus, press bare `s` to search project contents — not `prefix+s`, which is
Herdr's default shortcut for its own Settings screen. Viewer keys do not use the Herdr prefix.

The docs cover [installing and updating](docs/install.md),
[summoning the viewer](docs/summoning.md) (split vs. tab, opening at another directory, the
launcher, `--remote`), [external renderers](docs/renderers.md), and the
[keys reference](docs/keys.md).

## Configuration

An optional, **read-only** TOML config file lets you override the editor, renderer and opener
commands, startup toggles, tree layout, and keybindings. A fully commented
[`config.example.toml`](config.example.toml) ships in the plugin folder. Copy it as `config.toml`
into the directory printed by `herdr plugin config-dir herdr-file-viewer`, then uncomment the
settings you want.

The full reference covers the file location, precedence, every key, and `[keys]` remapping. Read it
in **[docs/configuration.md](docs/configuration.md)**. See your effective settings any time in the
`?` help overlay's **Settings** section.

## Windows

Native Windows is supported as a **preview** (install works the same way; the open actions use
`-windows` action ids and herdr's preview channel). WSL works today with zero extra setup. See
[docs/windows.md](docs/windows.md).

## Documentation

Full docs live in **[docs/](docs/README.md)**:

- **[Install & updating](docs/install.md)**: prebuilt vs. source, pinning a version, local-dev linking, and remote notices.
- **[Summoning the viewer](docs/summoning.md)**: the open actions, the idempotent launcher, split vs. tab, opening at another directory, and the `--remote` caveat.
- **[Usage guide](docs/usage.md)**: a feature-by-feature tour of the whole viewer.
- **[Keys & mouse](docs/keys.md)**: the complete key table, mouse gestures, and editor hand-off.
- **[Configuration](docs/configuration.md)**: the full `config.toml` reference and `[keys]` remapping.
- **[External renderers](docs/renderers.md)**: the optional `glow` / `delta` / `bat` integrations and the plain-text fallback.
- **[Windows (preview)](docs/windows.md)**: native-Windows specifics and WSL.
- **[Architecture](ARCHITECTURE.md)**: one in-process TUI owning both columns, the component map, and the core decisions.
- **[Security](SECURITY.md)**: the threat model for opening untrusted content, and how to report a vulnerability.

## Contributing

Bug reports and feature requests are welcome. [Open an
issue](https://github.com/smarzban/herdr-file-viewer/issues), or see
[CONTRIBUTING.md](CONTRIBUTING.md) to build, test, and submit a change.

## License

[MIT](LICENSE) © Saeed Marzban
