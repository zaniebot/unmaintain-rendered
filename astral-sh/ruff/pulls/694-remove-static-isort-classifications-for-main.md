```yaml
number: 694
title: Remove static isort classifications for __main__, disutils
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: isort-static
created_at: 2022-11-12T04:58:42Z
updated_at: 2022-11-20T01:59:35Z
url: https://github.com/astral-sh/ruff/pull/694
synced_at: 2026-01-12T15:55:05Z
```

# Remove static isort classifications for __main__, disutils

---

_@andersk_

isort lacks these overrides, and classifies them as third-party by default.

I suspect `disutils` was a typo for `distutils`, which is in the standard library but slated for removal in Python 3.12. However, at least for now, isort classifies it in the standard library.

---

_Comment by @charliermarsh on 2022-11-12 14:13_

FWIW I took these from [`classify-imports`](https://github.com/asottile/classify-imports/blob/34795c511c8f755ab32f83713f16aafa990fa61b/classify_imports.py#L39).

---

_Merged by @charliermarsh on 2022-11-12 14:13_

---

_Closed by @charliermarsh on 2022-11-12 14:13_

---

_Branch deleted on 2022-11-20 01:59_

---
