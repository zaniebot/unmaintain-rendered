```yaml
number: 77
title: Go to with vendored typeshed stubs
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-28T18:04:18Z
updated_at: 2025-07-02T11:59:00Z
url: https://github.com/astral-sh/ty/issues/77
synced_at: 2026-01-12T15:54:22Z
```

# Go to with vendored typeshed stubs

---

_@MichaReiser_

Navigating to a symbol that's defined in typeshed currently doesn't work when using the vendored typeshed because Red Knot can't create an URL that the client can display (because they're not actual files on disk). 

Possible options:

* Extract the typeshed stubs (or just that one file). This should work with all LSP clients
* Use a custom URL (e.g. `vendored://`) with a custom handler to show such a file (see https://github.com/astral-sh/ruff/pull/16901/files)



---

_Label `server` added by @MichaReiser on 2025-03-28 18:04_

---

_Renamed from "[red-knot] Go to with vendored typeshed stubs" to "Go to with vendored typeshed stubs" by @MichaReiser on 2025-05-07 15:13_

---

_Assigned to @ibraheemdev by @dhruvmanila on 2025-07-01 02:54_

---

_Closed by @ibraheemdev on 2025-07-02 11:58_

---
