```yaml
number: 8054
title: "Skip over parentheses when detecting `in` keyword"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/if
created_at: 2023-10-18T20:38:33Z
updated_at: 2023-10-18T23:13:59Z
url: https://github.com/astral-sh/ruff/pull/8054
synced_at: 2026-01-12T15:55:25Z
```

# Skip over parentheses when detecting `in` keyword

---

_@charliermarsh_

## Summary

Given an expression like `[x for (x) in y]`, we weren't skipping over parentheses when searching for the `in` between `(x)` and `y`.

Closes https://github.com/astral-sh/ruff/issues/8053.


---

_Review requested from @konstin by @charliermarsh on 2023-10-18 20:38_

---

_Label `bug` added by @charliermarsh on 2023-10-18 20:38_

---

_Label `formatter` added by @charliermarsh on 2023-10-18 20:38_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-18 22:40_

---

_@MichaReiser approved on 2023-10-18 23:13_

---

_Merged by @charliermarsh on 2023-10-18 23:13_

---

_Closed by @charliermarsh on 2023-10-18 23:13_

---

_Branch deleted on 2023-10-18 23:13_

---
