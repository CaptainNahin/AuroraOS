# Changelog

All notable changes to AuroraOS are documented in this file.

---

## [1.0.0] — 2026-09-05

### Inherited State
- **Backend Architecture**: Node.js HTTP/WebSocket loopback server on `127.0.0.1` (`server/index.mjs`, `jail.mjs`, `fsapi.mjs`, `settings.mjs`, `shell.mjs`, `sysinfo.mjs`, `static.mjs`).
- **Frontend Core & Apps**: TypeScript implementation for shell, desktop, dock, window manager, and eight built-in apps (`files.ts`, `terminal.ts`, `editor.ts`, `settings.ts`, `calculator.ts`, `calendar.ts`, `monitor.ts`, `about.ts`).
- **Missing Components**:
  - Zero stylesheets written (`src/styles/shell.css` and `src/styles/apps.css` were completely empty).
  - Zero automated tests.
  - Zero Windows Sandbox packaging or launcher automation.
  - Zero documentation.

---

### Implemented in this Release

#### 1. Visual Styling & Glassmorphic Design System
- **Shell Stylesheet (`src/styles/shell.css`)**:
  - Implemented macOS/glassmorphic window frames with active/inactive depth, backdrop blur (`backdrop-filter: blur(28px) saturate(180%)`), and fallback opaque treatments for low-power modes.
  - Implemented interactive traffic light controls (Close `#ff5f56`, Minimize `#ffbd2e`, Maximize `#27c93f`) with vector glyph overlays visible on cluster hover.
  - Implemented window state indicators: unsaved dirty dot centered in close button, snapping preview overlay with CSS transition animation.
  - Implemented workspace pager strip with smooth active dot indicators, mouse-wheel desktop switching, and keyboard navigation.
  - Implemented dock strip with parabolic magnification curve, running app pips, badge counters, and active bounce keyframes.
  - Implemented top menubar with system clock, status items (battery, network, sound), and active dropdown menus.
  - Implemented launcher full-screen overlay with search input, categorized grid layout, and keyboard focus traps.
  - Implemented slide-out Notification Centre panel and action toast overlays.
  - Implemented system state screens (`#aurora-boot`, `.boot-failure`, `.connection-banner`, `.shutdown-screen`).
- **Application Stylesheet (`src/styles/apps.css`)**:
  - **Files**: Dual grid and list views, crumb navigation bar, storage sidebar with pinned places, metadata inspector, and file drag-and-drop states.
  - **Terminal**: Monospace display, prompt rendering, ANSI color classes, command scrollback, and cursor blink options.
  - **Text Editor**: Pixel-perfect synchronized gutter and text line heights, line numbers, status bar statistics, and tab indent handling.
  - **Settings**: Categorized sidebar navigation, grid layouts, custom toggles (`.switch-input`), range sliders, and wallpaper preview pickers.
  - **Calculator**: Dual-mode grid (standard 4-function and scientific/expression keypad), historical tape tape, and overflow protection.
  - **Calendar**: Month grid layout with current-day highlight, scheduled event badges, event editor dialog, and multi-timezone world clock cards.
  - **Activity Monitor**: Real-time CPU gauge rings, memory breakdown meters, and process table rows.
  - **About**: System summary cards, hardware specifications, uptime readouts, and licence credits modal.
  - **Shared Primitives**: Button variants (primary, danger, ghost), text inputs, select dropdowns, and keyboard shortcut pills (`.kbd`).
  - **CSS Audit**: Verified zero missing classes across all frontend modules (audit report: 0 missing classes).

#### 2. Scripts & Build Toolchain
- Implemented `scripts/clean.mjs` for removing build outputs cleanly across platforms.
- Implemented `scripts/measure.mjs` calculating minified and gzipped bundle sizes, code-splitting chunks, and total distribution footprint.
- Implemented `scripts/test-interaction.mjs` performing end-to-end integration testing with disposable sandbox homes, token auth validation, and headless Microsoft Edge rendering.

#### 3. Automated Test Suite (78 Tests Passing)
- **`tests/jail.test.mjs` (19 tests)**: Path normalization, `..` traversal blocking, Windows drive/UNC/ADS colon denial, NUL and C0/C1 control character rejection, reserved device names (`CON`, `PRN`, `AUX`, `NUL`), and symlink escape detection.
- **`tests/shell.test.mjs` (30 tests)**: POSIX tokenization, quote escaping, metacharacter denial (`;`, `|`, `&`, ``` ` ```, `<`), allowlist validation, flag denial (`node -e`, `git -c`), builtins execution (`echo`, `pwd`, `help`, `whoami`, `uname`), and file redirection (`>`, `>>`).
- **`tests/settings.test.mjs` (9 tests)**: Schema defaults, coercion, slider clamping, string length/control character validation, atomic writes via temp file + rename, corrupted file recovery with `.invalid` preservation, and JSON import/export validation.
- **`tests/fsapi.test.mjs` (13 tests)**: Pure classification helpers, filesystem bootstrap, directory creation (`mkdir`), empty file touch, file read/write, unique naming, file rename, recursive tree copy, file move with self-nesting protection, trash and permanent deletion, search, and disk usage calculations.
- **`tests/frontend.test.mjs` (7 tests)**: Pure DOM utilities (`clamp`, `formatBytes`, `formatRelative`), shortcut canonicalization and formatting, launcher search score weighting, calculator decimal arithmetic (avoiding float precision errors) and expression parsing, terminal prefix tab-completion, files entry sorting, and calendar date parsing and event sanitization.

#### 4. Packaging & Platform Delivery
- Created `run-in-windows-sandbox.wsb`: Declarative Windows Sandbox manifest mounting repository read-only at `C:\auroraos` with vGPU acceleration enabled.
- Created `scripts/sandbox-logon.ps1`: Automated Sandbox startup script that detects or downloads Node.js, boots the loopback backend, and launches Microsoft Edge in fullscreen app mode.
- Created `scripts/build-release.ps1`: Release packaging pipeline that produces a self-contained release directory with optional portable Node.js embedding.
- Created `scripts/start-windows-sandbox.ps1`: Pre-flight validator checking Windows edition, CPU virtualization, and Windows Sandbox feature state before launching.
- Created `scripts/run-qemu.ps1`: Fallback VM launcher for testing bootable images with WHPX/TCG acceleration.
- Created `README-WINDOWS-SANDBOX.md`: Step-by-step user guide for enabling and running Windows Sandbox.

#### 5. Documentation
- Created `README.md`, `ARCHITECTURE.md`, `docs/UI.md`, `TROUBLESHOOTING.md`, `CHANGELOG.md`, `LICENSE`, and `LICENSES.md`.
