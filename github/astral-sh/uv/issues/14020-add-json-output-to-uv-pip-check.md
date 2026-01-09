---
number: 14020
title: Add json output to uv pip check
type: issue
state: open
author: detlevo
labels:
  - enhancement
assignees: []
created_at: 2025-06-13T11:53:16Z
updated_at: 2025-06-13T12:03:04Z
url: https://github.com/astral-sh/uv/issues/14020
synced_at: 2026-01-07T13:12:18-06:00
---

# Add json output to uv pip check

---

_Issue opened by @detlevo on 2025-06-13 11:53_

### Summary

The ease the integration of uv into IDE it would be beneficial to get the output of 'uv pip check' as a json structure. Each entry should give the package name of the broken package, the requirement of that package and the version that is currently installed. All that information is already shown but as textual output.

The option '--output-format text' should give the current output and '--output-format json' the new one. The default should be 'text'.

### Example

_No response_

---

_Label `enhancement` added by @detlevo on 2025-06-13 11:53_

---

_Comment by @zanieb on 2025-06-13 12:03_

Part of https://github.com/astral-sh/uv/issues/411

---
