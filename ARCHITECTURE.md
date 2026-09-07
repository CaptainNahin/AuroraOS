# AuroraOS Architecture & Technical Design

This document details the system design, communication protocols, security jail enforcement, and frontend compositor architecture of AuroraOS.

---

## 1. System Overview

AuroraOS implements the **Option B** architecture: a desktop operating environment running as a single-origin web application hosted on an isolated Node.js loopback backend, viewed through **Microsoft Edge in standalone App Mode** (`--app=http://127.0.0.1:<port>`).

```mermaid
graph TD
    subgraph HostMachine [Host Windows System]
        EdgeApp[Microsoft Edge App Mode<br/>Chromium Window without Browser Chrome]
        NodeBackend[Node.js Loopback Backend<br/>127.0.0.1:EPHEMERAL_PORT]
        HostFS[Host Filesystem Vault<br/>D:\os or %LOCALAPPDATA%\AuroraOS\home]
    end

    EdgeApp -->|HTTP + WebSocket with X-Aurora-Token| NodeBackend
    NodeBackend -->|Jail Confinement & Virtual Root| HostFS

    subgraph FrontendArchitecture [Frontend Compositor (TypeScript)]
        MainShell[Main Shell & MenuBar]
        WindowManager[Window Manager & Drag/Snap]
        DesktopSurface[Desktop & Wallpaper Canvas]
        DockBar[Dock & Magnification Strip]
        AppModules[Lazy Loaded App Modules<br/>Files, Terminal, Editor, Settings, etc.]
    end

    EdgeApp --- FrontendArchitecture
```

---

## 2. Loopback IPC & Authentication Design

### Single-Origin Binding
The backend server binds strictly to loopback addresses (`127.0.0.1`, `localhost`, `[::1]`). Any incoming connection containing an unexpected `Host` header is immediately rejected with `403 Forbidden`.

### Ephemeral Token Injection
To protect against cross-origin drive-by attacks from other local processes or rogue browser tabs:
1. Upon server startup, a cryptographically random 32-byte session token is generated (`crypto.randomBytes(32).toString('base64url')`).
2. When the browser requests `/` or `/index.html`, the backend reads `build/index.html` and atomically replaces the `/*__AURORA_BOOT__*/` comment with an inline assignment:
   ```javascript
   window.__AURORA_BOOT__ = {
     token: "...",
     version: "1.0.0",
     port: 7777,
     settings: { ... }
   };
   ```
3. Every subsequent HTTP request to `/api/*` (except `/api/health`) must provide this token via the `X-Aurora-Token` HTTP header.
4. Token matching uses `crypto.timingSafeEqual` to prevent timing side-channel analysis. Unauthenticated or invalid requests receive `401 Unauthorized`.

---

## 3. Security Model & Jail Confinement

AuroraOS enforces a zero-trust model between the frontend shell and the host operating system:

```
Virtual Path (AuroraOS)            Real Host Path
/                               -> D:\os\data\home
/Documents/notes.txt            -> D:\os\data\home\Documents\notes.txt
/../../Windows/System32 (Deny)  -> Blocked by Jail.resolve() (JailError)
```

### 1. Virtual Path Normalization & Jailing (`server/lib/jail.mjs`)
- All user-supplied filesystem paths are interpreted within a virtual root `/`.
- Path traversal sequences (`..`) are normalized strictly within the jail boundary and cannot escape above `/`.
- Paths containing NUL bytes (`\0`), Windows Alternate Data Streams (`:`), reserved device names (`CON`, `PRN`, `AUX`, `NUL`, `COM1..9`, `LPT1..9`), trailing dots or spaces, or control characters (`\x00` through `\x1f`, `\x7f`) are blocked before disk access.
- Symlinks pointing outside the vault root are recognized and access is denied.

### 2. Terminal Isolation (`server/lib/shell.mjs`)
- **No System Shell**: The terminal does **not** invoke `cmd.exe`, `powershell.exe`, or `/bin/sh`.
- **POSIX Tokenizer**: Input lines are parsed directly into strict `argv` arrays with quoting support.
- **Metacharacter Denial**: Operators such as `;`, `|`, `&`, ``` ` ```, and `<` trigger immediate syntax errors rather than subshell spawning.
- **Strict Program Allowlist**: Only explicit binaries (`node`, `git`, `tsc`) can be launched.
- **Dangerous Flag Deny-List**: Exec flags (`node -e`, `node -p`, `node --eval`) and Git config hook overrides (`git -c`, `git --exec-path`) are intercepted and rejected.

---

## 4. Frontend Architecture

### 1. Imperative DOM Rendering (`src/core/dom.ts`)
AuroraOS uses a lightweight, zero-dependency imperative DOM builder (`el()`) instead of heavy virtual-DOM or reactive frameworks:
- Text is assigned exclusively through `.textContent`, never `.innerHTML`, guaranteeing immunity to XSS injection from user files or terminal output.
- Windows are native DOM elements transformed using hardware-accelerated CSS `transform` and `backdrop-filter`.

### 2. Window Manager & Compositing (`src/shell/wm.ts`)
- **Z-Index Layering**: Managed stacking order with active window elevation.
- **Workspaces**: 6 virtual desktops; windows on inactive workspaces are hidden via `display: none` without unmounting state.
- **Snap Engine**: Real-time bounding calculation for left/right split and full-screen snapping with visual preview ghost.

### 3. Code Splitting & Lazy App Loading (`src/shell/registry.ts`)
- The main entry bundle (`aurora.js`) contains only the shell chrome, window manager, and app registry metadata.
- App implementations (`files.ts`, `terminal.ts`, `editor.ts`, etc.) are partitioned into individual chunks (`chunks/app-*.js`) via dynamic `import()`.
- Apps are loaded on demand upon first launch and cached in memory for instantaneous subsequent activations.
- Invariant 5.1 ensures zero app code is bundled into the main shell entry point.
