---
number: 9512
title: False negative with SIM108
type: issue
state: closed
author: NeilGirdhar
labels: []
assignees: []
created_at: 2024-01-14T07:25:50Z
updated_at: 2025-01-01T06:18:53Z
url: https://github.com/astral-sh/ruff/issues/9512
synced_at: 2026-01-10T01:22:49Z
---

# False negative with SIM108

---

_Issue opened by @NeilGirdhar on 2024-01-14 07:25_

```python
def main() -> None:
    with open('a'):
        for i in range(10):
            yrange = None
            integer_yrange: None | range
            if yrange is not None:
                integer_yrange = range(int(yrange[0]), int(yrange[1]) + 1)
            else:
                integer_yrange = None
            print(integer_yrange)
```
Does not produce use-ternary-operator.  Removing the function, context manager and loop does.

---

_Comment by @charliermarsh on 2024-01-14 15:13_

I believe this is https://github.com/astral-sh/ruff/issues/8106 -- it does flag it if you increase the line length: https://play.ruff.rs/c11cd4e9-a96c-40e6-ae60-6c735ab5bfb0

---

_Closed by @charliermarsh on 2024-01-14 15:13_

---

_Comment by @NeilGirdhar on 2025-01-01 04:07_

Sorry, but why does line length matter here?  I think the ternary expression is still an improvement even if it doesn't fit on one line.  Can we format it like so:
```python
if not isinstance(inference_result, RivalConfiguration):
    code = None
else:
    code = np.asarray(inference_result.pooling_output, dtype=np.float64)
```
becomes
```python
code = (np.asarray(inference_result.pooling_output, dtype=np.float64)
        if isinstance(inference_result, RivalConfiguration)
        else None)
```

---

_Comment by @charliermarsh on 2025-01-01 04:21_

(You should contribute to the discussion in the linked issue, which is open.)

---

_Comment by @NeilGirdhar on 2025-01-01 06:18_

Sorry, my mistake :)

---
