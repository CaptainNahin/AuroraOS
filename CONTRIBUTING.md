# 🤝 Contributing to AuroraOS

Thank you for your interest in contributing to **AuroraOS**! We are building a modern, lightweight, glassmorphic desktop environment that is evolving towards a bootable bare-metal Linux distribution.

Whether you're fixing a bug, adding a new application, improving performance, or working on our kernel and Wayland DRM compositor, we welcome your contributions.

---

## 🛠️ Development Setup

### 1. Prerequisites
- **Node.js**: Version 18.0.0 or later (Node 20 or 22 LTS recommended)
- **Git**: Installed and configured on your machine
- **A Modern Web Browser**: Google Chrome, Microsoft Edge, Brave, or Firefox

### 2. Fork & Clone
```bash
git clone https://github.com/<your-username>/auroraos.git
cd auroraos
npm install
```

### 3. Verify Local Setup
Ensure the build and tests pass on your machine before making changes:
```bash
npm run check
```
This runs:
1. `npm run typecheck` (TypeScript strict validation)
2. `npm run build` (Client bundle and icon bundling)
3. `npm test` (All 78 unit & integration tests)

---

## 🏃 Running the Development Server

Start the backend server and launch AuroraOS:
```bash
npm start
```
Then open `http://127.0.0.1:7777` in your browser.

To rebuild frontend assets automatically while editing files:
```bash
npm run dev
```

---

## 📐 Project Structure & Invariants

AuroraOS follows strict architectural invariants:

```
d:/os/
├── server/               # Node.js backend
│   ├── index.mjs         # Loopback HTTP & WebSocket server, session token lifecycle
│   └── lib/
│       ├── fs.mjs        # Sandboxed vault filesystem operations
│       ├── jail.mjs      # Virtual path canonicalization & symlink defense
│       ├── shell.mjs     # POSIX tokenized command execution & security filters
│       ├── settings.mjs  # Settings persistence & coercion
│       └── static.mjs    # Static asset serving & cache headers
│
├── src/                  # Frontend compositor (Vanilla TypeScript)
│   ├── core/             # Desktop shell, window manager, workspaces, dock, IPC
│   ├── apps/             # Built-in apps (browser, files, editor, calc, terminal, etc.)
│   └── styles/           # Pure CSS stylesheets (tokens.css, base.css, shell.css, apps.css)
│
├── tests/                # Automated test suite (Node.js test runner)
├── scripts/              # Build, audit, bundle measurement scripts
└── docs/                 # Architectural specifications
```

### Important Architectural Invariants
1. **Zero Runtime Dependencies**: The runtime relies entirely on standard Node.js libraries (`node:fs`, `node:http`, `node:crypto`, `node:path`) and browser web platform APIs. Do not add runtime dependencies to `dependencies` in `package.json`.
2. **Filesystem Confinement**: All file operations MUST go through the jail layer (`server/lib/jail.mjs`). No code may ever access arbitrary host paths without traversal verification.
3. **CSS Completeness**: All CSS classes referenced in TypeScript must exist in CSS stylesheets. Before submitting changes, run `npm run audit:css` to verify there are zero missing classes.
4. **Precision Math**: The Calculator app must avoid JavaScript floating-point drift (`0.1 + 0.2 = 0.3`). Any arithmetic improvements must preserve this decimal precision invariant.

---

## 🧪 Testing Checklist

Before opening a pull request, run the test gates:

```bash
# Strict TypeScript validation
npm run typecheck

# CSS audit (verifies 0 missing classes)
npm run audit:css

# Production build
npm run build:prod

# Automated test suite (78 tests)
npm test

# All-in-one check
npm run check
```

---

## 📝 Pull Request Workflow

1. **Branch Naming**:
   - `feat/feature-name` for new features or applications
   - `fix/bug-name` for bug fixes
   - `docs/doc-update` for documentation changes
   - `perf/optimization` for performance improvements
2. **Commit Messages**: Follow Conventional Commits format:
   - `feat: add split view to browser app`
   - `fix: correct window resize snap calculation`
   - `docs: update quickstart instructions`
3. **Open a PR**:
   - Provide a clear summary of what was changed and why.
   - Include before/after screenshots for any visual UI changes.
   - Confirm that `npm run check` passed cleanly.

---

## 💬 Code of Conduct

We are committed to providing a welcoming, inclusive, and harassment-free environment for everyone. Treat all contributors and community members with kindness and respect.
