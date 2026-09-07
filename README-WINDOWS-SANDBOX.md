# Running AuroraOS in Windows Sandbox

AuroraOS provides first-class support for **Windows Sandbox**, delivering a completely isolated, disposable desktop environment that leaves zero traces on your host machine.

---

## 1. Prerequisites

Windows Sandbox requires:
- **Windows 11 Pro, Enterprise, or Education** (or Windows 10 Pro/Enterprise build 18305+)
- **AMD64 (x64) architecture** with CPU virtualization enabled in BIOS/UEFI (Intel VT-x or AMD-V)
- At least 4 GB RAM (8 GB recommended) and 1 GB free disk space

> [!NOTE]
> Windows Home edition does not include Windows Sandbox. If you are running Windows Home, run AuroraOS directly on your machine using `npm start` (see section 4 below).

---

## 2. Enabling Windows Sandbox

If Windows Sandbox is not already enabled on your machine, enable it with a single PowerShell command:

1. Right-click the **Start Menu** and select **Terminal (Admin)** or **PowerShell (Admin)**.
2. Run the following command:
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName "Containers-DisposableClientVM" -NoRestart
   ```
3. Restart your computer when prompted.

You can verify the status at any time using:
```powershell
powershell -ExecutionPolicy Bypass -File .\scripts\start-windows-sandbox.ps1
```

---

## 3. Launching AuroraOS

### Quick Launch (Recommended)
Simply double-click the configuration file in File Explorer:
```
D:\os\run-in-windows-sandbox.wsb
```
or run from PowerShell:
```powershell
.\scripts\start-windows-sandbox.ps1
```

### What Happens Behind the Scenes:
1. Windows Sandbox boots a fresh, pristine Windows instance.
2. The repository at `D:\os` is mounted read-only at `C:\auroraos`.
3. The logon script (`scripts/sandbox-logon.ps1`) executes automatically:
   - Verifies Node.js (or automatically fetches the official standalone binary into `C:\node` if not pre-bundled).
   - Starts the AuroraOS loopback server on `http://127.0.0.1:7777`.
   - Opens **Microsoft Edge in App Mode** (`--app=http://127.0.0.1:7777`) maximized with all browser chrome stripped away.
4. Within **5 to 10 seconds**, the AuroraOS desktop appears on screen ready for interaction.

---

## 4. Local Windows Fallback (No Sandbox Required)

If you are running Windows Home, or prefer to run AuroraOS directly on your host without virtualization:

1. Open PowerShell or Command Prompt in the repository folder:
   ```powershell
   cd D:\os
   ```
2. Start the AuroraOS backend and launch Edge in app mode:
   ```powershell
   npm start
   ```
   or launch Edge in app mode explicitly:
   ```powershell
   node server/index.mjs --open
   ```
3. The server starts on an ephemeral loopback port, outputs the session URL and PID, and opens Edge in standalone app window mode.

---

## 5. Shutting Down

- **Inside Windows Sandbox:**
  Simply close the Windows Sandbox window or click the close button on the title bar. All temporary files, browser profiles, and state within the sandbox are immediately and permanently discarded by Windows.
- **Local Fallback:**
  Press `Ctrl+C` in the terminal running `node server/index.mjs`, or use the **Shut Down** item in the top-left Aurora menu.
