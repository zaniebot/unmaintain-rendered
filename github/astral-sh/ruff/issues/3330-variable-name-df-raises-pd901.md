---
number: 3330
title: "Variable name `df` raises PD901"
type: issue
state: open
author: janosh
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-03-03T22:56:46Z
updated_at: 2023-07-10T01:17:41Z
url: https://github.com/astral-sh/ruff/issues/3330
synced_at: 2026-01-07T13:12:14-06:00
---

# Variable name `df` raises PD901

---

_Issue opened by @janosh on 2023-03-03 22:56_

> PD901 `df` is a bad variable name. Be kinder to your future self.

`df` is the standard variable name for data frames in the `pandas`/`polars` communities and should be excluded from `PD901`.

---

_Comment by @charliermarsh on 2023-03-03 22:59_

Hmm, it is the exact intent of the rule in `pandas-vet`, but maybe we should remove it, it's considered "very opinionated", so it feels off to me that Ruff enables it whenever users enable `pandas-vet`.

https://github.com/deppen8/pandas-vet#very-opinionated-warnings

---

_Comment by @janosh on 2023-03-03 23:01_

Oops, should have read the docs. Was under the false impression `df` was being flagged on account of being only 2 letters.

---

_Comment by @charliermarsh on 2023-03-04 22:11_

@edgarrmondragon - Do you have an opinion on whether this rule stays or goes?

---

_Label `question` added by @charliermarsh on 2023-03-04 22:11_

---

_Referenced in [astral-sh/ruff#5485](../../astral-sh/ruff/pulls/5485.md) on 2023-07-03 17:28_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:17_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:17_

---

_Label `rule` added by @charliermarsh on 2023-07-10 01:17_

---
