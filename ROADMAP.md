# 🗺️ AuroraOS Engineering Roadmap

This document outlines the strategic vision, technical architecture, and implementation milestones for evolving **AuroraOS** from a high-performance web desktop environment into an **independent, bootable, bare-metal Linux distribution**.

---

## 🎯 The Vision: Why Bare-Metal AuroraOS?

Modern desktop operating systems face significant challenges:
1. **Resource Bloat**: Mainstream desktop environments (GNOME, Windows 11, KDE) frequently consume 2 GB to 4 GB of RAM merely sitting idle at the desktop.
2. **Legacy Abstractions**: Decades of legacy X11 protocols, synchronous IPC architectures, and fragmented toolkit layers (GTK, Qt, Win32) introduce micro-stutters and input latency.
3. **Visual Compromise**: Most lightweight desktop environments sacrifice modern visual aesthetics (real-time glassmorphism, fluid physics, cohesive design systems) for performance.

**AuroraOS solves this equation**:
By combining an ultra-lean Linux kernel, direct hardware rendering (DRM/KMS), and a unified TypeScript/CSS compositor, AuroraOS targets:
- **< 300 MB idle RAM consumption** on bare metal.
- **Sub-3-second cold boot** from UEFI firmware to interactive desktop.
- Uncompromised 60fps glassmorphic window compositing and desktop animations.

---

## 📊 Milestone Breakdown

```
┌────────────────────────────────────────────────────────────────────────┐
│ MILESTONE 1: Web Desktop & Virtual Jail                       ✅ DONE  │
├────────────────────────────────────────────────────────────────────────┤
│ MILESTONE 2: Native Desktop Shell & System Bridge           🔄 ACTIVE  │
├────────────────────────────────────────────────────────────────────────┤
│ MILESTONE 3: Direct DRM/KMS Wayland Compositor             🎯 UPCOMING │
├────────────────────────────────────────────────────────────────────────┤
│ MILESTONE 4: Independent Bootable Linux Distribution       🌟 ULTIMATE │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📍 Milestone 1: Web Desktop & Virtual Jail *(Completed)*

- [x] **Glassmorphic Compositor Engine**: Real-time backdrop blur, light/dark themes, dynamic accent palettes.
- [x] **Window Manager Geometry**: Draggable, resizable, minimizable, maximizable windows with traffic light indicators and magnetic edge snapping.
- [x] **Virtual Workspaces**: 6 independent desktops switchable via menu bar pager, mouse wheel, or global shortcuts.
- [x] **Sandboxed Filesystem Jail**: Safe, isolated vault filesystem with symlink-escape prevention, path sanitization, and soft-delete Trash.
- [x] **Built-in Application Suite**:
  - 🌐 Tabbed Web Browser with Speed Dial and cross-origin handling
  - 📁 Multi-pane Files Manager with instant folder/file creation
  - 📝 Desktop Code & Text Editor with line numbers and dirty state tracking
  - 🧮 Precision Decimal Calculator avoiding IEEE-754 floating-point drift
  - 💻 Sandboxed Terminal with POSIX tokenizer and allowlisted tools
  - ⚙️ System Settings control center
  - 📊 Real-time Activity Monitor (CPU, RAM, Processes)
  - 📅 World Clock & Calendar
- [x] **Zero Runtime Dependencies**: Pure Node.js standard library backend + Vanilla TypeScript frontend.
- [x] **Windows Sandbox Integration**: Disposable container boot with `.wsb` profile.

---

## 📍 Milestone 2: Native Desktop Shell & System Bridge *(In Progress)*

Transitioning AuroraOS from browser tabs to standalone native desktop runtimes across Windows, Linux, and macOS.

### Key Objectives
- [ ] **Lightweight Kiosk Packaging**:
  - Build Tauri / lightweight embedded Chromium kiosk binary (`< 30 MB` packaged distribution).
- [ ] **Native OS Filesystem Access**:
  - Implement user-granted directory mapping so users can mount local folders (`~/Documents`, `~/Projects`) into the Files app.
- [ ] **Hardware System Tray & Native Windowing**:
  - System tray icon with background service controls.
  - Native frameless window integration with OS-level window shadows.
- [ ] **Hardware Accelerated Media**:
  - Native video/audio decoding pass-through for smooth streaming in Aurora Browser.

---

## 📍 Milestone 3: Direct DRM/KMS Wayland Compositor *(Upcoming)*

Eliminating the host desktop environment entirely on Linux by running AuroraOS directly on the Linux Direct Rendering Manager (DRM) and Kernel Mode Setting (KMS).

```
┌───────────────────────────────────────────────────────────┐
│              AuroraOS Compositor (Frontend UI)            │
├───────────────────────────────────────────────────────────┤
│            Chromium Embedded / Wayland Surface            │
├───────────────────────────────────────────────────────────┤
│         Lightweight Kiosk Compositor (Cage / wlroots)     │
├───────────────────────────────────────────────────────────┤
│          Linux Kernel Mode Setting (DRM/KMS)              │
├───────────────────────────────────────────────────────────┤
│           GPU Hardware (Intel / AMD / NVIDIA Mesa)        │
└───────────────────────────────────────────────────────────┘
```

### Key Objectives
- [ ] **Zero-X11 Architecture**:
  - Boot directly to a minimal Wayland kiosk compositor (`cage` or customized `wlroots` shell).
  - No GDM, LightDM, or SDDM login manager overhead.
- [ ] **Hardware Input Pipeline**:
  - Native multi-touch gestures, smooth trackpad scrolling, and mouse acceleration via `libinput`.
- [ ] **Multi-Monitor Display Server**:
  - Dynamic display resolution detection, EDID parsing, and hot-plugging via `libdrm`.

---

## 📍 Milestone 4: Independent Bootable Linux Distribution *(The Grand Goal)*

The culmination of the project: downloading an `.iso` image, flashing it to a USB drive, and booting directly into AuroraOS on bare metal hardware.

### Architecture & Stack

| Layer | Technology | Rationale |
| :--- | :--- | :--- |
| **Kernel** | Minimal Linux LTS Kernel | High hardware compatibility, modern GPU/NVMe drivers, stripped of unnecessary enterprise/server modules. |
| **Base System** | Alpine Linux / Arch Minimal Base | Musl libc or minimal glibc; fast cold boot; minimal disk footprint (`< 1.5 GB` complete installation). |
| **Init & Daemon** | OpenRC or systemd-minimal | Instant parallel service initialization (`< 800ms`). |
| **Network** | `iwd` (Wi-Fi) + `systemd-networkd` | Eliminates heavy NetworkManager daemons while retaining robust WPA3/Enterprise support. |
| **Audio** | PipeWire + WirePlumber | Ultra-low latency pro-audio routing and Bluetooth audio profiles. |
| **Hardware Bridge** | Aurora Native D-Bus Daemon | Node.js / Rust system service communicating over D-Bus to control brightness, audio levels, battery stats, and Wi-Fi networks directly from AuroraOS Settings. |
| **Installer** | Aurora Web Installer | An intuitive graphical installer built directly as an AuroraOS app to format drives, setup EFI partitions, and install the OS to disk. |

### Development Timeline & Release Goals
- **v1.0 (Current)**: High-Performance Web Operating Environment.
- **v1.5**: Tauri / Desktop Kiosk Package with Native Host Bridges.
- **v2.0-Alpha**: Experimental Live Linux ISO booting to AuroraOS DRM/KMS kiosk.
- **v2.0-Beta**: Full D-Bus hardware controls (Wi-Fi, Bluetooth, Audio, Power) inside AuroraOS Settings.
- **v2.0-GA**: Production Bootable AuroraOS Linux Distribution with GUI disk installer.

---

## 🤝 How to Join the Development

If you are a systems programmer, kernel hacker, Rust/C developer, or frontend UI specialist:
- Join discussions in our GitHub Issues and Discussions.
- Check out the issues tagged with `roadmap`, `kernel`, `compositor`, and `hardware-bridge`.
- Read [CONTRIBUTING.md](CONTRIBUTING.md) to get your local development environment configured.
