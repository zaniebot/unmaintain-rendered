```yaml
number: 17427
title: "Bump `pysource-minimize` (and other dependencies?) in `py-fuzzer`'s `uv.lock` file"
type: issue
state: closed
author: AlexWaygood
labels:
  - internal
  - help wanted
  - testing
assignees: []
created_at: 2025-04-16T11:35:49Z
updated_at: 2025-05-15T14:47:39Z
url: https://github.com/astral-sh/ruff/issues/17427
synced_at: 2026-01-12T15:54:55Z
```

# Bump `pysource-minimize` (and other dependencies?) in `py-fuzzer`'s `uv.lock` file

---

_@AlexWaygood_

There was just a new release of `pysource-minimize`, one of the libraries used by our [`py-fuzzer`](https://github.com/astral-sh/ruff/tree/main/python/py-fuzzer) script. We should try to upgrade the `pysource-minimize` pin in the script's `uv.lock` file (as long as the script still works as intended), and see if we can bump any other dependencies there as well.

https://github.com/15r10nk/pysource-minimize/releases/tag/v0.8.0

---

_Label `help wanted` added by @AlexWaygood on 2025-04-16 11:35_

---

_Label `internal` added by @AlexWaygood on 2025-04-16 11:35_

---

_Label `testing` added by @AlexWaygood on 2025-04-16 11:35_

---

_Closed by @AlexWaygood on 2025-05-15 14:47_

---
