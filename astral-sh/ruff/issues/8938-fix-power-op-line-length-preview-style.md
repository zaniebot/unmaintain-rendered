```yaml
number: 8938
title: "`fix_power_op_line_length` preview style"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-12-01T03:25:08Z
updated_at: 2023-12-02T00:35:37Z
url: https://github.com/astral-sh/ruff/issues/8938
synced_at: 2026-01-10T11:09:51Z
```

# `fix_power_op_line_length` preview style

---

_Issue opened by @MichaReiser on 2023-12-01 03:25_

Implement Black's [`fix_power_op_line_length`](https://github.com/psf/black/pull/3942) style as a preview style in Ruff. 

```python
b = 1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1**1
```

Should be formatted to 

```python
b = (
    1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
    ** 1
)
```

Notice the whitespace around the power op which we currently omit. This likely requires using `if_group_breaks` to only show the whitespace when the group breaks.

---

_Renamed from "`fix_power_op_line_length`" to "`fix_power_op_line_length` preview style" by @MichaReiser on 2023-12-01 03:25_

---

_Label `formatter` added by @MichaReiser on 2023-12-01 03:25_

---

_Label `preview` added by @MichaReiser on 2023-12-01 03:25_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-12-01 03:25_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-12-01 16:33_

---

_Closed by @MichaReiser on 2023-12-02 00:35_

---
