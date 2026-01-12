```yaml
number: 2888
title: "[docs] Add docs for `flake8-errmsg`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: docs/flake8-errmsg
created_at: 2023-02-14T04:53:10Z
updated_at: 2023-02-15T00:01:02Z
url: https://github.com/astral-sh/ruff/pull/2888
synced_at: 2026-01-12T15:55:11Z
```

# [docs] Add docs for `flake8-errmsg`

---

_@edgarrmondragon_

Related:

- https://github.com/charliermarsh/ruff/issues/2646


---

_@not-my-profile reviewed on 2023-02-14 05:47_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_errmsg/rules.rs`:11 on 2023-02-14 05:47_

I know that the rule is currently named RawStringInException but I think it should be renamed from raw to direct since raw string usually refers to strings such as `r"this"`.

So "raw usage" should be reworded here. Perhaps:

Checks for the direct supplying of a string literal to an exception constructor.

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_errmsg/rules.rs`:53 on 2023-02-14 05:48_

`less code` is a bit ambiguous here ... your source code actually got more

---

_@not-my-profile reviewed on 2023-02-14 05:48_

---

_Comment by @not-my-profile on 2023-02-14 05:50_

This is again a case where I think it would make sense to merge all of these rules into one (see also #2714).

They all check for the same problem ... I don't think there is any scenario where a user may want to enable one of these but not the others.

---

_Merged by @charliermarsh on 2023-02-14 23:21_

---

_Closed by @charliermarsh on 2023-02-14 23:21_

---

_Branch deleted on 2023-02-15 00:01_

---
