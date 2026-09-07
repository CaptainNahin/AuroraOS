# Troubleshooting Guide for AuroraOS

This guide covers common issues encountered when building, launching, or running AuroraOS, along with their verified resolutions.

---

## 1. Port Conflicts (EADDRINUSE)

### Symptom:
When running `npm start` or `node server/index.mjs`, you see:
```
aurora: port 7777 is already in use. Choose another with --port.
```

### Fix:
1. **Specify a different port**:
   ```powershell
   node server/index.mjs --port 8888
   ```
2. **Or let AuroraOS pick an ephemeral free port automatically**:
   ```powershell
   node server/index.mjs --port 0 --open
   ```
3. **Terminate orphaned backend processes**:
   Find the PID using `netstat` and terminate it cleanly:
   ```powershell
   Get-NetTCPConnection -LocalPort 7777 | Select-Object -ExpandProperty OwningProcess | Stop-Process -Force
   ```
   > [!IMPORTANT]
   > Never run blanket `taskkill /IM node.exe /F` as it may terminate unrelated developer tooling or IDE services.

---

## 2. Windows Sandbox Fails to Launch

### Symptom:
Double-clicking `run-in-windows-sandbox.wsb` fails with an error like "Windows Sandbox could not be initialized" or "The hypervisor is not present".

### Causes & Fixes:
1. **Windows Sandbox feature is disabled**:
   Open an Administrator PowerShell window and run:
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName "Containers-DisposableClientVM" -NoRestart
   ```
   Reboot your PC to finalize installation.
2. **CPU Virtualization disabled in BIOS/UEFI**:
   Reboot into your BIOS setup and enable **Intel Virtualization Technology (VT-x)** or **AMD SVM / AMD-V**.
3. **Windows Home Edition**:
   Windows Home does not support Windows Sandbox. Use the local fallback instead:
   ```powershell
   npm start
   ```

---

## 3. Blank White Screen on Boot

### Symptom:
Edge opens in app mode, but the window remains blank white or shows `#aurora-boot` indefinitely.

### Causes & Fixes:
1. **Shell has not been built**:
   If running from a fresh clone, run the production build first:
   ```powershell
   npm run build:prod
   ```
2. **Opening `build/index.html` directly via `file://` protocol**:
   AuroraOS requires the Node.js loopback backend to serve runtime tokens and inject system settings into `/*__AURORA_BOOT__*/`. Direct `file://` execution is explicitly blocked for security.
   Always launch via `npm start` or `node server/index.mjs`.
3. **Browser console inspection**:
   Press `F12` or `Ctrl+Shift+I` in Edge app mode to open Developer Tools and view any runtime errors in the console.

---

## 4. Terminal Commands Failing

### Symptom:
Running commands like `bash`, `powershell`, or `curl` in AuroraOS Terminal outputs `command not found` or `external commands are disabled`.

### Root Cause:
By design, AuroraOS Terminal does **not** invoke a system shell (cmd/bash/powershell) and strictly denies shell metacharacters (`;`, `|`, `&`, ``` ` ```).
Only built-in commands (`help`, `ls`, `cat`, `echo`, `mkdir`, `rm`, `df`, `free`, `ps`, `uptime`) and allowlisted developer binaries (`node`, `git`, `tsc`) are executable.
- To view available commands:
  ```
  help
  ```
- If the server was launched with `--no-external`, all external programs are disabled. Restart without `--no-external` if external command execution is desired.

---

## 5. Settings Not Persisting

### Symptom:
Changes made in Settings (e.g. wallpaper, theme, dock position) reset after restarting the server.

### Causes & Fixes:
1. **Running inside Windows Sandbox**:
   Windows Sandbox is disposable by definition. Every sandbox session starts from a fresh, pristine environment.
2. **Jail file corruption**:
   Settings are saved atomically to `/.aurora/settings.json`. If this file is corrupted, AuroraOS automatically recovers by copying it to `.invalid` and loading safe schema defaults. Check `/.aurora/settings.json.invalid` in the Files app.

---

## 6. High DPI Scaling & Font Blurriness

### Symptom:
On 4K or high-refresh displays, UI text appears blurry or too small.

### Fix:
1. Open **Settings → Display** inside AuroraOS and adjust the **Display Scale** slider.
2. Alternatively, adjust zoom inside Microsoft Edge using `Ctrl + +` or `Ctrl + -`.
3. Verify your Windows Display scaling is set to 100%, 125%, or 150% in native Windows Display Settings.
