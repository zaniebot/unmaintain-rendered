---
number: 2146
title: Improve rendering of ReST hyperlinks in docstrings
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-21T16:36:00Z
updated_at: 2025-12-21T20:18:56Z
url: https://github.com/astral-sh/ty/issues/2146
synced_at: 2026-01-10T01:52:52Z
---

# Improve rendering of ReST hyperlinks in docstrings

---

_Issue opened by @AlexWaygood on 2025-12-21 16:36_

E.g. our [very own docstrings](https://github.com/astral-sh/ruff/blob/b4c2825afdd8c1010c3a5859521629d2d1e0a6df/crates/ty_vendored/ty_extensions/ty_extensions.pyi#L115-L120) in `ty_extensions.pyi` currently use [standard ReST syntax for hyperlinks](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#hyperlinks), but these aren't rendered in their intended way at all currently when you hover over one of these APIs:

<img width="1852" height="448" alt="Image" src="https://github.com/user-attachments/assets/f9343eb0-73af-489c-961c-0b9376b504b1" />

Pyright also doesn't seem to do a great job here, though, so this maybe shouldn't be a priority.

---

_Label `server` added by @AlexWaygood on 2025-12-21 16:36_

---

_Label `wish` added by @AlexWaygood on 2025-12-21 16:36_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-21 20:18_

---
