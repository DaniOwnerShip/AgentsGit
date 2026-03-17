---
title: Avoid @electron/remote
impact: HIGH
tags: electron, security, ipc, architecture
---

# Rule: Avoid @electron/remote
**ID:** ELEC-004  
**Severity:** HIGH

## Description
The `@electron/remote` module is deprecated, slow, and introduces security risks. It should not be used in modern Electron applications.

## Audit Logic
- Search for `require('@electron/remote')` or `import ... from '@electron/remote'`.
- **Error if:** The module is used anywhere in the codebase.
- **Error if:** It is used to manipulate BrowserWindow or other main process APIs from the renderer.

## Remediation
Move any logic requiring main process access to `main.js` and expose it via secure IPC:

```javascript
// main.js
ipcMain.handle('create-window', () => {
  // create BrowserWindow here
});

// renderer (via preload)
ipcRenderer.invoke('create-window');