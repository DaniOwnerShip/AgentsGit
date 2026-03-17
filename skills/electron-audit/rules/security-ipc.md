---
title: Secure IPC Communication
impact: HIGH
tags: electron, security, ipc, preload
---

# Rule: Secure IPC Communication
**ID:** ELEC-002  
**Severity:** HIGH

## Description
Do not expose entire Node modules (like `fs` or `child_process`) via `contextBridge`. You must expose only specific, gated functions and validate all communication channels.

## Audit Logic
- Review `preload.js` files.
- **Error if:** An entire module is exposed: `contextBridge.exposeInMainWorld('api', require('fs'))`.
- **Error if:** Unsafe IPC patterns are used without validation.

## Recommendation
- Prefer `ipcRenderer.invoke` over `ipcRenderer.send` for request-response patterns.

## Remediation
```javascript
// In preload.js (Secure Way)
contextBridge.exposeInMainWorld('electronAPI', {
  saveFile: (data) => ipcRenderer.invoke('save-file-channel', data)
});