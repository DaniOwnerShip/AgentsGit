---
name: subagent-electron-review
description: "Sub-agent that audits an Electron app using electron-best-practices skill."
license: MIT
metadata:
  author: tu-nombre
  version: "1.0"
---

## Purpose
Audit Electron main and renderer processes for security and IPC best practices.

## What to Receive
- project_path: path to the Electron project

## What to Do
1. Load the skill `electron-best-practices`.
2. For each rule in the skill:
   - Apply the `Audit Logic` to the files in `project_path`.
   - Record any violations.
3. Return a structured envelope with all findings.

## Rules
- Always use the skill rules; do not skip any.
- Return an envelope with `status` and `issues`.
