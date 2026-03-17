---
title: Context Isolation & Node Integration
impact: CRITICAL
tags: electron, security, context-isolation
---

# Rule: Context Isolation & Node Integration
**ID:** ELEC-001  
**Severity:** CRITICAL

## Description
The renderer process (React) must not have direct access to Node.js APIs. This prevents an XSS attack from escalating into full Remote Code Execution (RCE) on the user's operating system.

## Audit Logic
- Locate `new BrowserWindow({ webPreferences: { ... } })`.
- **Error if:** `contextIsolation` is `false` or missing.
- **Error if:** `nodeIntegration` is `true`.

## Remediation
```javascript
// Correct Implementation
const win = new BrowserWindow({
  webPreferences: {
    contextIsolation: true,
    nodeIntegration: false,
    sandbox: true
  }
});