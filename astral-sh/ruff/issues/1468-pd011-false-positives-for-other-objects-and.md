```yaml
number: 1468
title: PD011 false positives for other objects and method calls
type: issue
state: closed
author: guyrosin
labels:
  - rule
assignees: []
created_at: 2022-12-30T08:42:53Z
updated_at: 2023-01-01T22:06:16Z
url: https://github.com/astral-sh/ruff/issues/1468
synced_at: 2026-01-12T15:54:41Z
```

# PD011 false positives for other objects and method calls

---

_@guyrosin_

PD011 (UseOfDotValues) triggers for `.values` attribute access and `.values()` method calls for objects that are not dataframes. Examples:

```py
{"hello": 1}.values
{"hello": 1}.values()
```

---

_Comment by @edgarrmondragon on 2022-12-31 09:46_

Unfortunately this an shared issue with `pandas-vet`: https://github.com/deppen8/pandas-vet/issues/74#issuecomment-582329625.

I think the best we can do is not trigger on the second case (i.e. method call).

---

_Label `rule` added by @charliermarsh on 2022-12-31 18:12_

---

_Closed by @charliermarsh on 2023-01-01 22:05_

---

_Comment by @charliermarsh on 2023-01-01 22:06_

I added some heuristics that should help with this case in #1538. But it will still be a problem in some cases.

---
