---
number: 6500
title: "Formatter: Expands function call after multi-line string"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-08-11T11:56:38Z
updated_at: 2023-08-16T07:11:31Z
url: https://github.com/astral-sh/ruff/issues/6500
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Expands function call after multi-line string

---

_Issue opened by @konstin on 2023-08-11 11:56_

```python
"""
{}
""".format("hi")
```
should be formatted as
```python
"""
{}
""".format(
    "hi"
)
```
This depends on the string content, e.g.
```python
"""{}""".format("hi")
```
does not get expanded.

---

_Label `formatter` added by @konstin on 2023-08-11 11:56_

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-08-11 11:56_

---

_Comment by @charliermarsh on 2023-08-15 02:16_

It looks like this no longer happens in Black's preview style: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ACHAFhdAD2IimZxl1N_Wk8Jgdzhwf57J25o-MMyJFDdKTbHVMsgIS4qWrqO3MV-q3-8HoAKJvbhIFbv_2jUfXQyafZeaz7n_OCmk3vdd7F3rCF3dZvDEseZ-a6DkyQAYnRsQVfRpAwAAXSIAQAAADwHzGqxxGf7AgAAAAAEWVo=

---

_Comment by @charliermarsh on 2023-08-15 02:58_

Okay, I think it's https://github.com/psf/black/issues/256 and https://github.com/psf/black/pull/1879. Closing as I think this is reasonable given it's in preview.

---

_Closed by @charliermarsh on 2023-08-15 02:58_

---

_Comment by @charliermarsh on 2023-08-15 03:02_

However, there's another behavior implemented in the linked PRs that we _don't_ have: https://github.com/astral-sh/ruff/issues/6581.

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:11_

---
