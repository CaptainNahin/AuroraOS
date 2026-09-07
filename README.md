<div align="center">

# 🌌 AuroraOS

### *A Next-Generation, Glassmorphic Desktop Operating Environment*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?logo=node.js&logoColor=white)](https://nodejs.org/)
[![Zero Runtime Dependencies](https://img.shields.io/badge/Runtime%20Dependencies-0-success.svg)](package.json)
[![Tests Passing](https://img.shields.io/badge/Tests-78%20Passing-brightgreen.svg)](tests/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

<p align="center">
  <b>Fast. Fluid. Sandboxed. Zero Bloat.</b><br/>
  AuroraOS delivers an ultra-smooth, macOS-inspired glassmorphic desktop experience running on a privilege-separated loopback engine.<br/>
  Test it in your favorite web browser today — watch it evolve into an independent bare-metal Linux OS tomorrow.
</p>

[Quickstart](#-quickstart--running-locally) •
[Features](#-features--core-apps) •
[Architecture](#-system-architecture) •
[Roadmap to Bare-Metal OS](#-roadmap-the-road-to-a-bare-metal-bootable-os) •
[Contributing](#-contributing)

---

</div>

## 💡 What is AuroraOS?

**AuroraOS** reimagines the modern desktop. Instead of heavy virtual machines or bloated multi-gigabyte runtimes, AuroraOS pairs a **sub-millisecond Node.js privilege-separated backend** with a **pure Vanilla TypeScript glassmorphic compositor**.

- ⚡ **Cold Boot in < 500ms**: Minimal memory footprint (~50MB idle RSS).
- 🎨 **State-of-the-Art Glassmorphism**: Dynamic blur filters, physics-driven window snapping, parabolic dock magnification, and 6 virtual workspaces.
- 🛡️ **Filesystem Jail Vault**: Isolated storage sandbox protecting host files against accidental edits or symlink escapes.
- 📦 **Zero Runtime Dependencies**: The entire core engine uses pure Node.js standard libraries and native web platform APIs. No framework overhead.

---

## ⚡ Quickstart — Running Locally

You can clone, build, and test AuroraOS in **under 60 seconds**.

### 1. Clone & Install
```bash
# Clone the repository
git clone https://github.com/<your-username>/auroraos.git
cd auroraos

# Install development dependencies (esbuild, typescript, lucide-static)
npm install
```

### 2. Verify System Integrity
Run the automated test suite and type-checker to ensure everything compiles cleanly:
```bash
npm run check
```
*(Runs strict TypeScript check, production bundle builder, and all 78 automated test cases)*

### 3. Launch the Server
```bash
npm start
```
The server will bind to `http://127.0.0.1:7777` (or an ephemeral loopback port) and generate a secure, cryptographically random session token.

### 4. Experience AuroraOS

Open any modern browser and navigate to:
```
http://127.0.0.1:7777
```

#### 🌟 Pro-Tip: Run as a Native Desktop App (No Browser Chrome)

To hide the browser address bar and tabs for an authentic desktop OS feeling, run:

- **Google Chrome**:
  ```bash
  chrome.exe --app=http://127.0.0.1:7777 --start-maximized
  ```
- **Microsoft Edge**:
  ```bash
  msedge.exe --app=http://127.0.0.1:7777 --start-maximized
  ```
- **Brave**:
  ```bash
  brave.exe --app=http://127.0.0.1:7777 --start-maximized
  ```

#### 🛡️ Run Inside Windows Sandbox (100% Isolated)
If you are on Windows 10/11 Pro or Enterprise, simply double-click:
```
run-in-windows-sandbox.wsb
```
Or execute in PowerShell:
```powershell
.\scripts\start-windows-sandbox.ps1
```
*AuroraOS will boot in a disposable Hyper-V container with zero host trace.*

---

## ✨ Features & Core Apps

AuroraOS comes out of the box with a complete ecosystem of daily-driver apps and desktop features:

```
 AuroraOS Desktop Environment
 ├── 🪟 Glassmorphic Window Compositor
 │   ├── Real-time backdrop blur & saturation
 │   ├── Physics-driven drag & magnetic edge-snapping (Left / Right / Maximize preview)
 │   ├── Traffic light controls with unsaved dirty-state indicators
 │   └── 6 Independent Virtual Workspaces with menu bar pager
 │
 ├── 🚀 Parabolic Magnification Dock
 │   ├── Dynamic zoom physics & active app running pips
 │   └── System tray status indicators (Clock, Battery, Network, Sound)
 │
 └── 📱 Built-in Application Suite
     ├── 🌐 Aurora Browser       (Multi-tab, Speed Dial, proxy bypass, instant search)
     ├── 📁 Files Manager         (Sandboxed vault, tree view, search, trash recovery)
     ├── 📝 Text & Code Editor    (Line numbers, word wrap, dirty state, instant save)
     ├── 🧮 Precision Calculator  (No float drift: 0.1+0.2=0.3, expression tape & history)
     ├── 💻 Sandboxed Terminal    (POSIX tokenizer, path completion, allowlisted safety)
     ├── ⚙️ System Settings       (7 accent colors, wallpapers, light/dark themes, sound)
     ├── 📊 Activity Monitor      (Live CPU gauges, RAM distribution, process manager)
     └── 📅 Calendar & Clock      (Multi-timezone world clocks & interactive calendar)
```

### Detailed App Breakdown

| Application | Description |
| :--- | :--- |
| **🌐 Web Browser** | Full-fledged tabbed web browser with Speed Dial favorites, omnibox search (DuckDuckGo fallback), URL validation, and cross-origin security mitigation. |
| **📁 Files Manager** | Multi-column sandboxed filesystem browser with instant folder/file creation, selection, rename, breadcrumb navigation, and soft-delete Trash. |
| **📝 Text Editor** | Clean desktop text & code editing surface with desktop toolbar (`New`, `Open`, `Save`, `Save As`), synchronized line gutters, and dirty state tracking. |
| **🧮 Calculator** | Dual-mode arithmetic workstation with high-precision decimal math (eliminating IEEE-754 floating-point inaccuracies), algebraic expression evaluator, and calculation history. |
| **💻 Terminal** | Secure command-line environment featuring POSIX argument tokenization, shell history, glob matching, and strict prevention of subshell command injections. |
| **⚙️ Settings** | Centralized control panel to switch between Light/Dark themes, 7 accent palettes, custom desktop wallpapers, dock magnification scales, and accessibility options. |
| **📊 Activity Monitor** | Live telemetry monitoring showing CPU utilization, memory allocation meters, uptime counters, and running process tables. |
| **📅 Calendar & Clock** | Interactive month calendar with event scheduling and world clock cards across UTC, New York, London, Tokyo, and Sydney. |

---

## ⌨️ Global Keyboard Shortcuts

| Shortcut | Action |
| :--- | :--- |
| <kbd>Alt</kbd> + <kbd>Space</kbd> | Toggle Global Spotlight Launcher |
| <kbd>Ctrl</kbd> + <kbd>N</kbd> | Open New Window |
| <kbd>Ctrl</kbd> + <kbd>W</kbd> | Close Focused Window |
| <kbd>Ctrl</kbd> + <kbd>M</kbd> | Minimize Focused Window |
| <kbd>Ctrl</kbd> + <kbd>S</kbd> | Save Document (in Editor) |
| <kbd>Ctrl</kbd> + <kbd>1</kbd> ... <kbd>6</kbd> | Switch Directly to Workspace 1 through 6 |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>→</kbd> | Switch to Next Workspace |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>←</kbd> | Switch to Previous Workspace |
| <kbd>Alt</kbd> + <kbd>Tab</kbd> | Cycle Open Windows |
| <kbd>F11</kbd> | Toggle Fullscreen Mode |

---

## 🏗️ System Architecture

AuroraOS is engineered around modularity and security:

```
┌─────────────────────────────────────────────────────────────┐
│                       Host System                           │
│  (Google Chrome / Edge / Brave / Electron / Linux DRM Kiosk)│
└──────────────────────────────┬──────────────────────────────┘
                               │ HTTP / WebSocket IPC
                               │ (X-Aurora-Token Authenticated)
┌──────────────────────────────▼──────────────────────────────┐
│             AuroraOS Loopback Engine (Node.js)              │
├──────────────────────────────┬──────────────────────────────┤
│ 🛡️ Jail Filesystem Sandbox   │ 💻 Process & Shell Guardian  │
│  - Traversal sanitization    │  - POSIX tokenization        │
│  - Symlink escape prevention │  - Allowlisted executables   │
│  - Atomic JSON persistence   │  - Subshell denial           │
└──────────────────────────────┴──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│               Aurora Compositor (TypeScript)                │
├─────────────────────────────────────────────────────────────┤
│  • Window Manager & Geometry     • Virtual Workspaces       │
│  • Magnetic Snap Engine          • Dynamic Glassmorphic CSS │
│  • Parabolic Magnification Dock  • Modular App Registry     │
└─────────────────────────────────────────────────────────────┘
```

For complete technical documentation, review [ARCHITECTURE.md](ARCHITECTURE.md).

---

## 🚀 Roadmap: The Road to a Bare-Metal Bootable OS

AuroraOS is not merely a web desktop demo — it is an active engineering journey towards an **independent, lightweight, bare-metal Linux distribution**.

```
  [Phase 1] ✅ Web Desktop & Virtual Jail (Current)
      ↓
  [Phase 2] 🔄 Standalone Native Shell (Tauri / Electron / Chromium Kiosk)
      ↓
  [Phase 3] 🎯 Direct DRM/KMS Wayland Compositor (Zero X11 / Zero GNOME bloat)
      ↓
  [Phase 4] 🌟 Independent Bootable Linux Distribution (Live ISO + Installer)
```

### Phase 1: Web Desktop Environment *(Completed)*
- Pure TypeScript compositor with glassmorphic visuals.
- Filesystem sandbox jail and secure privilege-separated loopback backend.
- Full suite of built-in productivity apps (Browser, Files, Editor, Calculator, Terminal, Settings, Activity Monitor).
- Zero external runtime dependencies.

### Phase 2: Native Desktop Shell *(In Progress)*
- Single-executable distribution for Linux, Windows, and macOS using Tauri / lightweight Chromium embedded runtime.
- Native filesystem access with user-granted directory permissions.
- Direct hardware integration (volume hotkeys, battery telemetry, native clipboard).

### Phase 3: Direct DRM/KMS Wayland Compositor *(Upcoming)*
- Run AuroraOS directly on bare metal without requiring heavy legacy display servers (X11) or resource-hungry desktop environments (GNOME / KDE / XFCE).
- Implement a lightweight Wayland compositor (via `wlroots` / `cage`) that renders the AuroraOS compositor directly to the Linux Direct Rendering Manager (`/dev/dri/card0`).
- Boot-to-desktop latency targeted at **under 3 seconds**.

### Phase 4: AuroraOS Linux Distribution *(The Grand Goal)*
- **Kernel**: Minimal LTS Linux Kernel tailored for fast boot, ACPI power management, and modern GPU acceleration (Intel, AMD, NVIDIA).
- **Init System**: High-speed, lightweight init (Alpine musl/OpenRC or minimal systemd).
- **Hardware Daemon**: Native D-Bus service bridging Wi-Fi (`iwd`), Bluetooth (`bluez`), Audio (`pipewire`), and display brightness straight into AuroraOS Settings.
- **Bootable ISO**: Live USB installer with a Calamares-inspired graphical AuroraOS web installer for installing to bare-metal SSDs/NVMe drives.

*See our dedicated [ROADMAP.md](ROADMAP.md) for architectural milestones and technical specifications.*

---

## 🧪 Testing & Verification

AuroraOS maintains strict engineering standards:

```bash
# Run all 78 automated unit and integration tests
npm test

# Verify that every CSS token and rule is complete (0 missing classes)
npm run audit:css

# Run production build optimization
npm run build:prod

# Inspect bundle and release sizes
npm run measure
```

---

## 🤝 Contributing

We welcome contributions from developers, designers, and Linux enthusiasts worldwide! Whether it's adding new apps, improving the compositor physics, or working on the Linux kernel / DRM bootstrap, your help is appreciated.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'feat: add AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on code style, testing gates, and contribution guidelines.

---

## 📜 License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for more information.

---

<div align="center">
  <sub>Built with ❤️ by the AuroraOS Community. Star us on GitHub if you believe in the future of lightweight desktop operating systems!</sub>
</div>
