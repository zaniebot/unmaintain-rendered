---
number: 17356
title: "`noqa: TC003` not equivalent to `ignore = [\"TC003\"]` (TCH -> TC name change?)"
type: issue
state: closed
author: dmyersturnbull
labels:
  - needs-mre
assignees: []
created_at: 2025-04-11T16:20:58Z
updated_at: 2025-04-22T08:50:58Z
url: https://github.com/astral-sh/ruff/issues/17356
synced_at: 2026-01-07T13:12:16-06:00
---

# `noqa: TC003` not equivalent to `ignore = ["TC003"]` (TCH -> TC name change?)

---

_Issue opened by @dmyersturnbull on 2025-04-11 16:20_

### Summary

The renaming of TCH to TC resulted in some extremely confusing behavior, originally seen in Ruff 0.11.2.

**Summary:** `ruff: noqa: TC003` works but `ignore = ["TC003"]` does not. Related to #16763 (reported in #14810).

**In action:** [minimal working example](https://play.ruff.rs/908a0930-7512-4e1e-b87e-a6e7a592fd3e) in 0.11.5.

Playing around on `play.ruff.rs`, it only occurs with _both_ `TCH` and `TC` in `select`. In fact, it depends on specific set of options that are unlikely to occur in the wild. Opening this issue only in case it's a symptom of another issue.

### Issue seen in Ruff 0.11.2

In Ruff 0.11.2, having `preview` enabled and only `TCH` in `select` is sufficient.

```toml
# File: pyproject.toml

[tool.ruff]
preview = true

[tool.ruff.lint]
select = ["TCH"]
ignore = ["TC003"]
```

```python
# File: src/pkg/__init__.py

# Violation: TC003: Move [...] into a type-checking block
from os import PathLike
```

<b>Doesn't suppress:</b>

```toml
# File: pyproject.toml
[tool.ruff.lint]
select = ["TCH"]
ignore = ["TC003"]
```

<b>Does suppress:</b>

```python
# File: src/pkg/core.py
# ruff: noqa: TC003
from os import PathLike
```


### Version

_No response_

---

_Comment by @Daverball on 2025-04-13 13:21_

I can't reproduce your findings. Your minimal repro doesn't match the configuration you provided and has nothing to do with rule redirects. You can get the same behavior with any rule if you put a narrow code in `select` and a broad code in `ignore`.

E.g. the following configuration will not suppress F401 errors, even though `F` is part of `ignore`.
```toml
[tool.ruff.lint]
select = ["F401"]
ignore = ["F"]
```
but the reverse definitely works, even with rule redirects. I've tried every possible combination of rule redirects and they all work on your example:

https://play.ruff.rs/3a8fbde0-862d-4637-9e91-c89814b47972
```toml
select = ["TC"]
ignore = ["TCH003"]
```

https://play.ruff.rs/293d84fa-2418-4d25-ab94-08d4e2fcf9a5
```toml
select = ["TCH"]
ignore = ["TC003"]
```

https://play.ruff.rs/c830de15-22bc-4080-9810-27045e83f338
```toml
select = ["TCH"]
ignore = ["TCH003"]
```

---

_Label `needs-mre` added by @ntBre on 2025-04-14 13:32_

---

_Closed by @MichaReiser on 2025-04-22 08:50_

---
