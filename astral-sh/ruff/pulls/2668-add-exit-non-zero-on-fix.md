```yaml
number: 2668
title: "Add `--exit-non-zero-on-fix`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - cli
assignees: []
merged: true
base: main
head: charlie/show-fix
created_at: 2023-02-08T19:05:33Z
updated_at: 2023-02-08T23:47:38Z
url: https://github.com/astral-sh/ruff/pull/2668
synced_at: 2026-01-12T15:55:09Z
```

# Add `--exit-non-zero-on-fix`

---

_@charliermarsh_

When enabled, we exit zero if any errors are fixed, even if the fixes result in there being no remaining lint violations.

Closes #2633.

---

_Label `configuration` added by @charliermarsh on 2023-02-08 19:05_

---

_Label `cli` added by @charliermarsh on 2023-02-08 19:05_

---

_Merged by @charliermarsh on 2023-02-08 19:27_

---

_Closed by @charliermarsh on 2023-02-08 19:27_

---

_Branch deleted on 2023-02-08 19:27_

---

_Comment by @stefanv on 2023-02-08 23:47_

> When enabled, we exit zero if any errors are fixed, even if the fixes result in there being no remaining lint violations.

Presumably, "when enabled, we exit non-zero if any errors are fixed...".

Thanks for implementing this, @charliermarsh! :pray:

---
