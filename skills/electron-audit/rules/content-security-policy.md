---
title: Missing Content Security Policy (CSP)
impact: HIGH
tags: electron, security, csp
---

# Rule: Missing Content Security Policy (CSP)
**ID:** ELEC-005  
**Severity:** HIGH

## Description
Without a strict Content Security Policy (CSP), the application is vulnerable to script injection attacks that could bypass security protections.

## Audit Logic
- Review `.html` files or HTTP response headers.
- **Error if:** No CSP is defined.
- **Error if:** CSP contains unsafe directives such as `'unsafe-inline'` or `'unsafe-eval'`.

## Remediation

Avoid using a static CSP. Instead, define different policies for development and production.

### Option 1: Inject CSP from Electron main process (recommended)

```javascript
win.webContents.session.webRequest.onHeadersReceived((details, callback) => {
  const isDev = process.env.NODE_ENV === 'development';

  const csp = isDev
    ? "default-src 'self'; script-src 'self' 'unsafe-eval' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; connect-src 'self' http://localhost:* ws://localhost:* http://192.168.*.*; img-src 'self' data:;"
    : "default-src 'self'; script-src 'self'; style-src 'self'; connect-src 'self' http://192.168.X.X:*; img-src 'self' data:;";

  callback({
    responseHeaders: {
      ...details.responseHeaders,
      'Content-Security-Policy': [csp]
    }
  });
});