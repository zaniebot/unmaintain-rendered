---
number: 11789
title: "(🎁) format `type: ignore` and other pragmas"
type: issue
state: open
author: KotlinIsland
labels:
  - formatter
  - style
assignees: []
created_at: 2024-06-07T00:24:54Z
updated_at: 2024-11-18T00:13:32Z
url: https://github.com/astral-sh/ruff/issues/11789
synced_at: 2026-01-07T13:12:15-06:00
---

# (🎁) format `type: ignore` and other pragmas

---

_Issue opened by @KotlinIsland on 2024-06-07 00:24_

```diff
- 1 + ""  # type:ignore[ unsafe-variance,operator]
+ 1 + ""  # type: ignore[unsafe-variance, operator]
```

- Like https://github.com/psf/black/issues/2601 but actually implemented

---

_Comment by @DetachHead on 2024-06-07 00:28_

some other examples: `pyright: ignore`, `mypy: disable-error-code=foo` and `pyright: reportFoo=true`

---

_Label `formatter` added by @charliermarsh on 2024-06-07 00:48_

---

_Label `style` added by @MichaReiser on 2024-06-07 05:11_

---

_Comment by @InSyncWithFoo on 2024-11-13 02:04_

This should be partly handled by [`RUF104`](https://github.com/astral-sh/ruff/pull/14111).

---

_Comment by @Avasam on 2024-11-18 00:13_

I guess this is not quite a duplicate of https://github.com/astral-sh/ruff/issues/10160 since you're also asking to format the code list, and I only mentioned the colon

---

_Referenced in [astral-sh/ruff#16401](../../astral-sh/ruff/issues/16401.md) on 2025-08-16 17:11_

---
