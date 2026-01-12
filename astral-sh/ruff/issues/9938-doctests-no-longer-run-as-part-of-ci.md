```yaml
number: 9938
title: Doctests no longer run as part of CI
type: issue
state: closed
author: MichaReiser
labels:
  - ci
assignees: []
created_at: 2024-02-12T08:34:02Z
updated_at: 2024-02-12T09:18:59Z
url: https://github.com/astral-sh/ruff/issues/9938
synced_at: 2026-01-12T15:54:49Z
```

# Doctests no longer run as part of CI

---

_@MichaReiser_

https://github.com/astral-sh/ruff/pull/9921 migrated CI to nextest but without adding an explicit command to run the doctests (nextest [can't run doctests](https://github.com/nextest-rs/nextest/issues/16))

We should update our CI pipeline to run doctests as well.

---

_Label `ci` added by @MichaReiser on 2024-02-12 08:34_

---

_Closed by @MichaReiser on 2024-02-12 09:18_

---
