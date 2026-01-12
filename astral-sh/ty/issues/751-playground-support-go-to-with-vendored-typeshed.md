```yaml
number: 751
title: "[playground] Support go to with vendored typeshed stubs"
type: issue
state: closed
author: ibraheemdev
labels:
  - playground
assignees: []
created_at: 2025-07-02T12:03:20Z
updated_at: 2025-07-26T18:33:39Z
url: https://github.com/astral-sh/ty/issues/751
synced_at: 2026-01-12T15:54:23Z
```

# [playground] Support go to with vendored typeshed stubs

---

_@ibraheemdev_

https://github.com/astral-sh/ruff/pull/19057 implemented this for the LSP server but does not have WASM support.

---

_Label `playground` added by @ibraheemdev on 2025-07-02 12:03_

---

_Comment by @ibraheemdev on 2025-07-02 12:04_

I tried naively adding WASM support but it looks like there's something deeper missing to get the editor to open a new tab.

---

_Renamed from "[playground] Support to with vendored typeshed stubs" to "[playground] Support go to with vendored typeshed stubs" by @carljm on 2025-07-02 17:07_

---

_Comment by @MichaReiser on 2025-07-07 07:18_

Yeah, this is requires a bit more work because what files exists is currently tied to for which files we show tabs. We would need to split the internal state into *these are the project files* and *these are the files for which we know the content*

---

_Closed by @MichaReiser on 2025-07-26 18:33_

---
