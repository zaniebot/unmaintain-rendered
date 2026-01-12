```yaml
number: 189
title: Path handling on non-case sensitive systems
type: issue
state: open
author: MichaReiser
labels:
  - cli
  - server
assignees: []
created_at: 2025-02-26T15:12:14Z
updated_at: 2025-11-18T16:10:21Z
url: https://github.com/astral-sh/ty/issues/189
synced_at: 2026-01-12T15:54:22Z
```

# Path handling on non-case sensitive systems

---

_@MichaReiser_

The project file discovery and the `Files` interner assume that two paths that only different in casing are two unique paths. However, this is not true on all file systems and it could result in Red Knot considering two files as being different even though they're the same on disk. 

We should test how Red Knot handles paths with different casing on case-insensitive file systems (looking at you Windows). That also includes project file discovery.

Related to https://github.com/astral-sh/ruff/issues/13691

---

_Renamed from "[red-knot] Path handling on non-path sensitive systems" to "[red-knot] Path handling on non-case sensitive systems" by @carljm on 2025-02-26 17:16_

---

_Comment by @MichaReiser on 2025-03-14 09:57_

https://github.com/astral-sh/ruff/pull/16521 makes the module resolver case sensitive. It also adds a test demonstrating where our watcher gets out of sync with the `Files` state on case-insensitive systems (the test is currently ignored because of it and fixing this PR should make this test pass)

The main outstanding issue is that `Files` is case sensitive but it doesn't use case-sensitive system calls. However, how we fix this depends on a UX question: Should paths passed to the Red Knot CLI or in configurations be case sensitive on all platforms or only on case-sensitve platforms. Depending on that, we may need to make `Files` case-sensitive or respect the platform's case sensitivity and we may have to do the same for all `SystemPath` methods that compare paths (`starts_with`, `ends_with`, `equals`, ...)

---

_Renamed from "[red-knot] Path handling on non-case sensitive systems" to "Path handling on non-case sensitive systems" by @MichaReiser on 2025-05-07 15:26_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-08-08 14:26_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-08 14:26_

---

_Label `cli` added by @carljm on 2025-11-13 00:49_

---

_Label `server` added by @carljm on 2025-11-13 00:49_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-11-14 08:11_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:11_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
