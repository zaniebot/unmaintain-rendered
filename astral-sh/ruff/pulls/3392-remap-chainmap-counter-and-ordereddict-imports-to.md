```yaml
number: 3392
title: Remap ChainMap, Counter, and OrderedDict imports to collections
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pep-585
created_at: 2023-03-07T23:48:42Z
updated_at: 2023-03-07T23:53:37Z
url: https://github.com/astral-sh/ruff/pull/3392
synced_at: 2026-01-12T15:55:12Z
```

# Remap ChainMap, Counter, and OrderedDict imports to collections

---

_@charliermarsh_

`ChainMap` and `Counter` were erroneously mapped to `collections.abc` (instead of `collections`), and `OrderedDict` was missing entirely.

See: #3388.

---

_Label `bug` added by @charliermarsh on 2023-03-07 23:48_

---

_Merged by @charliermarsh on 2023-03-07 23:53_

---

_Closed by @charliermarsh on 2023-03-07 23:53_

---

_Branch deleted on 2023-03-07 23:53_

---
