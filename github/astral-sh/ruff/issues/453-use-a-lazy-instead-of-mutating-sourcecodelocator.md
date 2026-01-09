---
number: 453
title: "Use a `Lazy` instead of mutating `SourceCodeLocator` everywhere"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - internal
assignees: []
created_at: 2022-10-18T21:25:49Z
updated_at: 2022-10-26T15:27:49Z
url: https://github.com/astral-sh/ruff/issues/453
synced_at: 2026-01-07T13:12:14-06:00
---

# Use a `Lazy` instead of mutating `SourceCodeLocator` everywhere

---

_Issue opened by @charliermarsh on 2022-10-18 21:25_

We pass around `&mut checker` in a bunch of places because `SourceCodeLocator` lazily initializes itself. Could we instead use an actual `Lazy` to implement that behavior?


---

_Label `good first issue` added by @charliermarsh on 2022-10-18 21:25_

---

_Label `internal` added by @charliermarsh on 2022-10-18 21:25_

---

_Referenced in [astral-sh/ruff#472](../../astral-sh/ruff/pulls/472.md) on 2022-10-26 15:21_

---

_Closed by @charliermarsh on 2022-10-26 15:27_

---
