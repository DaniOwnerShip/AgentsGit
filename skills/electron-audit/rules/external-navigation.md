---
title: Restricted Navigation & External Links
impact: MEDIUM
tags: electron, security, navigation, external-links
---

# Rule: Restricted Navigation & External Links
**ID:** ELEC-003  
**Severity:** MEDIUM

## Description
The application should not allow navigation to external URLs inside the app window. External links must be opened in the system's default browser.

## Audit Logic
- Search for `will-navigate` events or `setWindowOpenHandler`.
- **Error if:** Navigations to untrusted origins are not intercepted and denied.

## Remediation
```javascript
const { shell } = require('electron');

const ALLOWED_ORIGINS = ['https://tu-app.com', 'https://api.tu-app.com'];

win.webContents.setWindowOpenHandler(({ url }) => {
  const { origin } = new URL(url);

  if (ALLOWED_ORIGINS.includes(origin)) {
    return { action: 'allow' };
  }

  shell.openExternal(url);
  return { action: 'allow' };  // SOLO PRUEBAS!!!!!!!!!!!!!!!!!!!!!!!! eliminar
  //return { action: 'deny' }; 
}); 
