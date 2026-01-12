```yaml
number: 1499
title: Improve F811 range for function and class definitions
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: improve-F811-location
created_at: 2022-12-31T06:03:01Z
updated_at: 2022-12-31T12:42:19Z
url: https://github.com/astral-sh/ruff/pull/1499
synced_at: 2026-01-12T05:36:31Z
```

# Improve F811 range for function and class definitions

---

_Pull request opened by @harupy on 2022-12-31 06:03_

This PR improves F811 range for function and class definitions using `helpers::identifier_range`.


### Current

```
> cargo run -- --select F811 --show-source resources/test/fixtures/pylint/used_prior_global_declaration.py
resources/test/fixtures/pylint/used_prior_global_declaration.py:156:1: F811 Redefinition of unused `f` from line 150
    |
156 | / def f():
157 | |     global x
158 | |     print(f"{x=}")
    | |__________________^ F811
    |
```

### Improved

```
> cargo run -- --select F811 --show-source resources/test/fixtures/pylint/used_prior_global_declaration.py
resources/test/fixtures/pylint/used_prior_global_declaration.py:156:5: F811 Redefinition of unused `f` from line 150
    |
156 | def f():
    |     ^ F811
    |
```



---

_Merged by @charliermarsh on 2022-12-31 12:42_

---

_Closed by @charliermarsh on 2022-12-31 12:42_

---
