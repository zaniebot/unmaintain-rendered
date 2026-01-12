```yaml
number: 2271
title: mark some fixers as sometimes-fixable
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: sometimes
created_at: 2023-01-27T17:51:56Z
updated_at: 2023-01-27T23:23:33Z
url: https://github.com/astral-sh/ruff/pull/2271
synced_at: 2026-01-12T04:52:00Z
```

# mark some fixers as sometimes-fixable

---

_Pull request opened by @spaceone on 2023-01-27 17:51_

A lot of fixers tell they are `always` fixable while they are only sometimes fixable.

```
$ ruff --explain PLR1722
use-sys-exit

Code: PLR1722 (Pylint)

Autofix is always available.
           ^^^^^^

Message formats:

* Use `sys.exit()` instead of `{name}`
```

My rust knowledge is not enough to introduce a `SometimesAutofixableViolation` so I downgrade it to regular `Violations`.

---

_Merged by @charliermarsh on 2023-01-27 23:23_

---

_Closed by @charliermarsh on 2023-01-27 23:23_

---
