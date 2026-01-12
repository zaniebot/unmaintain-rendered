```yaml
number: 2840
title: "[docs] Add docs for the entirety of `flake8-builtins`"
type: pull_request
state: merged
author: Sawbez
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs
created_at: 2023-02-13T02:35:12Z
updated_at: 2023-02-14T04:34:17Z
url: https://github.com/astral-sh/ruff/pull/2840
synced_at: 2026-01-12T15:55:11Z
```

# [docs] Add docs for the entirety of `flake8-builtins`

---

_@Sawbez_

This adds documentation for `A001`, `A002`, and `A003`. If you have any tweaks you would like then I can make them.

---

_Comment by @sbrugman on 2023-02-13 17:34_

False positive from clippy. 

Clippy can be configured to accept this CamelCase: `clippy.toml`:
```toml
doc-valid-idents = ["StackOverflow", ".."]
```

---

_Merged by @charliermarsh on 2023-02-14 04:30_

---

_Closed by @charliermarsh on 2023-02-14 04:30_

---

_Comment by @charliermarsh on 2023-02-14 04:34_

Thank you so much!

---

_Label `documentation` added by @charliermarsh on 2023-02-14 04:34_

---
