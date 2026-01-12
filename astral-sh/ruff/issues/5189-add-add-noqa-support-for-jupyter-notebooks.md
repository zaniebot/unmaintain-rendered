```yaml
number: 5189
title: "Add `--add-noqa` support for Jupyter notebooks"
type: issue
state: open
author: dhruvmanila
labels:
  - suppression
  - notebook
assignees: []
created_at: 2023-06-19T18:30:49Z
updated_at: 2024-03-22T11:38:38Z
url: https://github.com/astral-sh/ruff/issues/5189
synced_at: 2026-01-12T15:54:45Z
```

# Add `--add-noqa` support for Jupyter notebooks

---

_@dhruvmanila_

_No description provided._

---

_Label `core` added by @dhruvmanila on 2023-06-19 18:32_

---

_Label `noqa` added by @dhruvmanila on 2023-06-19 18:32_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-20 05:42_

---

_Comment by @janosh on 2023-07-12 21:04_

I have a notebook with one violation:

> 10:88:36: FA102 Missing `from __future__ import annotations`, but uses PEP 585 collection

I ran

```
ruff . --add-noqa
```

which somehow resulted in

> Added 171 noqa directives.

which made all notebooks in directory unparsable. Maybe best to bail on this operation until this feature is implemented rather than break notebooks.

---

_Comment by @charliermarsh on 2023-07-12 21:08_

Oh weird, I assumed they were just skipped. Will take a look.

---

_Unassigned @dhruvmanila by @dhruvmanila on 2023-09-12 11:14_

---

_Label `core` removed by @dhruvmanila on 2024-03-22 11:38_

---

_Label `notebook` added by @dhruvmanila on 2024-03-22 11:38_

---
