```yaml
number: 192
title: "mdtest: implement auto-update for inline `# error: ` message assertions"
type: issue
state: open
author: carljm
labels:
  - testing
  - internal
assignees: []
created_at: 2025-02-24T19:44:54Z
updated_at: 2025-11-18T16:10:21Z
url: https://github.com/astral-sh/ty/issues/192
synced_at: 2026-01-12T15:54:22Z
```

# mdtest: implement auto-update for inline `# error: ` message assertions

---

_@carljm_

### Description

It can be painful to manually update all the asserted error messages if the diagnostic message changes.

One way to mitigate this pain is by using full diagnostic snapshotting, but this currently puts the assertion in a separate file from the test (#16255) and even if it were inline, may be "overkill" for many tests, that hurts readability of the test.

---

_Renamed from "[red-knot] mdtest: implement auto-update for inline `# error: ` message assertions" to "mdtest: implement auto-update for inline `# error: ` message assertions" by @MichaReiser on 2025-05-07 15:26_

---

_Label `testing` added by @AlexWaygood on 2025-05-11 07:26_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:26_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-13 16:01_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
