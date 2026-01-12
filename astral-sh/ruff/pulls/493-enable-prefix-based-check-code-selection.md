```yaml
number: 493
title: Enable prefix-based check code selection
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/prefix-codes
created_at: 2022-10-28T01:56:08Z
updated_at: 2022-10-28T22:19:59Z
url: https://github.com/astral-sh/ruff/pull/493
synced_at: 2026-01-12T15:55:04Z
```

# Enable prefix-based check code selection

---

_@charliermarsh_

Resolves #325.

---

_Comment by @charliermarsh on 2022-10-28 02:00_

\cc @andersk 

---

_Comment by @andersk on 2022-10-28 02:17_

In Flake8, the most specific prefix wins, whether it’s a select or an ignore. In this implementation, ignores always win, so you can’t override a broad `--ignore=E5` with a specific `--select=E50`.

---

_Comment by @charliermarsh on 2022-10-28 02:25_

Ah ok. We can do that. I assume that ties break in favor of `--ignore`? Such that `--select A100 --ignore A100` ignores A100?

---

_Comment by @andersk on 2022-10-28 02:28_

Yeah, `--select` wins if it’s longer, `--ignore` wins if it’s longer or equal.

https://github.com/PyCQA/flake8/blob/7dfe99616fc2f07c0017df2ba5fa884158f3ea8a/src/flake8/style_guide.py#L171-L177

---

_Merged by @charliermarsh on 2022-10-28 22:19_

---

_Closed by @charliermarsh on 2022-10-28 22:19_

---

_Branch deleted on 2022-10-28 22:19_

---
