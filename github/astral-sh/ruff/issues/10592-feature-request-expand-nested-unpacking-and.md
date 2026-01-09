---
number: 10592
title: "Feature request: Expand nested unpacking and unpacking of single inline element"
type: issue
state: open
author: Avasam
labels:
  - rule
assignees: []
created_at: 2024-03-26T03:00:29Z
updated_at: 2025-11-09T18:29:59Z
url: https://github.com/astral-sh/ruff/issues/10592
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: Expand nested unpacking and unpacking of single inline element

---

_Issue opened by @Avasam on 2024-03-26 03:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
After running RUF005 in https://github.com/mhammond/pywin32/pull/2230 , I was left with some nested unpacking
![image](https://github.com/astral-sh/ruff/assets/1350584/b8d98537-e759-4105-aaa5-ed0f7a72eb31)

I feel like the following case should be detectable and flagged:
```python
# from
foo(*{bar})
foo(*[bar])
foo(*(bar,))
foo(*[bar, *bar2])
foo(*(bar, *bar2))
foo(*[*bar2, bar])
foo(*(*bar2, bar))

# to
foo(bar)  # Not a safe fix from *{bar}
foo(bar)
foo(bar)
foo(bar, *bar2)
foo(bar, *bar2)
foo(*bar2, bar)
foo(*bar2, bar)
```
(same with assignments and possibly more cases I haven't thought of)

Edit: Removed sets since https://github.com/astral-sh/ruff/issues/10592#issuecomment-2785004944
> Elements may be deduplicated and sets are unordered

---

_Label `rule` added by @MichaReiser on 2024-03-26 07:38_

---

_Referenced in [mhammond/pywin32#2230](../../mhammond/pywin32/pulls/2230.md) on 2024-12-31 03:24_

---

_Comment by @Avasam on 2025-04-08 01:19_

Kinda looks like https://docs.astral.sh/ruff/rules/unnecessary-spread/, but for single-star unpacking `*`

---

_Comment by @InSyncWithFoo on 2025-04-08 01:31_

The cases with sets should not be flagged, for two reasons: Elements may be deduplicated and sets are unordered.

---

_Comment by @Avasam on 2025-04-08 01:33_

> The cases with sets should not be flagged, for two reasons: Elements may be deduplicated and sets are unordered.

Good point.
A single-item literal set still works.

A multi-item set couldn't be autofixed (but still flagged! It's probably a code-smell anyway).

---

_Comment by @InSyncWithFoo on 2025-04-08 01:49_

Even single-item sets cannot be fixed, as the element might be unhashable (in which case the error would be masked).

---

_Referenced in [astral-sh/ruff#18668](../../astral-sh/ruff/issues/18668.md) on 2025-06-17 19:29_

---
