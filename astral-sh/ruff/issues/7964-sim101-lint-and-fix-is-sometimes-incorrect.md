```yaml
number: 7964
title: "`SIM101` lint and fix is sometimes incorrect"
type: issue
state: closed
author: Zac-HD
labels: []
assignees: []
created_at: 2023-10-15T12:58:34Z
updated_at: 2023-10-17T13:43:50Z
url: https://github.com/astral-sh/ruff/issues/7964
synced_at: 2026-01-12T15:54:47Z
```

# `SIM101` lint and fix is sometimes incorrect

---

_@Zac-HD_

The `SIM101` rule suggests replacing `isinstance(a. b) or isinstance(a, c)` with `isinstance(a, (b, c))` - but the lint and autofix also trigger when there is an intermediate term in the `or`-expression.  This is a problem because code can (and does) rely on short-circuiting evaluation in some such cases.  For example, `ruff --isolated --select=SIM101 --fix demo.py` will break:

```python
x = isinstance("", int) or True or isinstance("", undefined_name)
# x = isinstance("", (int, undefined_name)) or True  # incorrect fix!
```

I found this while experimenting with `ruff` in `shed`, which [has a similar fixer](https://github.com/Zac-HD/shed/blob/6bc10985fc2e22f5bd95539e388ca75b3c3da363/src/shed/_codemods.py#L459-L474).

---

_Comment by @zanieb on 2023-10-15 15:58_

Hey, thanks for the issue! This fix as marked unsafe and will not be applied by default in the upcoming release. See #7901 and #7769 for details.

---

_Closed by @zanieb on 2023-10-15 15:58_

---

_Comment by @Zac-HD on 2023-10-17 13:43_

Actual fix was in https://github.com/astral-sh/ruff/pull/7798 🙂 

---
