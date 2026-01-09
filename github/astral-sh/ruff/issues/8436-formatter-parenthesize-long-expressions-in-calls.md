---
number: 8436
title: "Formatter: Parenthesize long expressions in calls"
type: issue
state: closed
author: zanieb
labels:
  - formatter
  - preview
  - style
assignees: []
created_at: 2023-11-02T04:44:19Z
updated_at: 2023-11-29T03:02:53Z
url: https://github.com/astral-sh/ruff/issues/8436
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Parenthesize long expressions in calls

---

_Issue opened by @zanieb on 2023-11-02 04:44_

Stable formatting of calls with a long expression results in a wrap that reduces readability

Unformatted:

```python
if True:
    ___ = dict(
        x="---",
        y="----------------------------------------------------------" if True else None,
        z="---",
    )
```

Formatted:

```python
if True:
    ___ = dict(
        x="---",
        y="----------------------------------------------------------"
        if True
        else None,
        z="---",
    )
```

Adding parenthesis results in more readable code. Black's [preview style does so](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-parentheses-management).

```python
if True:
    ___ = dict(
        x="---",
        y=(
            "----------------------------------------------------------"
            if True
            else None
        ),
        z="---",
    )
```

With a shorter line, Ruff retains parenthesis when the lines are collapsed:

```python
if True:
    ___ = dict(
        x="---",
        y=("---------------------------------------------" if True else None),
        z="---",
    )
```

However, these parenthesis should be removed as they are superfluous.

Black's preview style **will not** remove these parenthesis for calls, but probably should.

Note Black's preview style will remove these parenthesis for dict literals, see https://github.com/astral-sh/ruff/issues/8437 but will not for list literals, see #8438 

See also https://github.com/psf/black/issues/620 https://github.com/psf/black/pull/3440 https://github.com/psf/black/issues/3510

---

_Label `formatter` added by @zanieb on 2023-11-02 04:44_

---

_Label `preview` added by @zanieb on 2023-11-02 04:44_

---

_Label `style` added by @zanieb on 2023-11-02 04:44_

---

_Referenced in [astral-sh/ruff#6935](../../astral-sh/ruff/issues/6935.md) on 2023-11-02 04:45_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-02 05:06_

---

_Referenced in [astral-sh/ruff#8437](../../astral-sh/ruff/issues/8437.md) on 2023-11-02 05:10_

---

_Referenced in [astral-sh/ruff#8438](../../astral-sh/ruff/issues/8438.md) on 2023-11-02 05:15_

---

_Renamed from "Formatter: Parenthesize long expressions in dicts and calls" to "Formatter: Parenthesize long expressions in calls" by @zanieb on 2023-11-02 05:15_

---

_Referenced in [astral-sh/ruff#8357](../../astral-sh/ruff/issues/8357.md) on 2023-11-02 05:20_

---

_Comment by @MichaReiser on 2023-11-29 03:02_

I merge this into https://github.com/astral-sh/ruff/issues/8438 because conditional expression should be parenthesized in call contexts (as far as I understand)

---

_Closed by @MichaReiser on 2023-11-29 03:02_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-11-29 03:02_

---
