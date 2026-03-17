---
title: Limit Preload API Exposure
impact: HIGH
tags: electron, security, preload, ipc
---

# Rule: Limit Preload API Exposure
**ID:** ELEC-006  
**Severity:** HIGH

## Description
Expose only the minimal, necessary APIs from the preload script to the renderer. Avoid exposing objects that allow direct Node.js access or unrestricted IPC channels.

## Audit Logic
- Review `preload.js` scripts.
- **Error if:** Any object exposes more than the strictly required methods.
- **Error if:** Node.js modules or raw IPC channels are exposed globally.

## Remediation
Use `contextBridge` to expose only safe functions:

```javascript
contextBridge.exposeInMainWorld('api', {
  saveFile: (data) => ipcRenderer.invoke('save-file'),
  getConfig: () => ipcRenderer.invoke('get-config')
});