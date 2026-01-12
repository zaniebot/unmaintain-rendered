```yaml
number: 1707
title: Add more backticks to flake8-pytest-style error messages
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: add-backticks-flake8-pytest-style
created_at: 2023-01-07T03:32:59Z
updated_at: 2023-01-07T03:55:25Z
url: https://github.com/astral-sh/ruff/pull/1707
synced_at: 2026-01-12T05:36:32Z
```

# Add more backticks to flake8-pytest-style error messages

---

_Pull request opened by @harupy on 2023-01-07 03:32_

_No description provided._

---

_@harupy reviewed on 2023-01-07 03:36_

---

_Review comment by @harupy on `src/registry.rs`:3532 on 2023-01-07 03:36_

```suggestion
                format!("Use a regular `assert` instead of unittest-style `{assertion}`")
```

---

_@charliermarsh reviewed on 2023-01-07 03:36_

---

_Review comment by @charliermarsh on `src/registry.rs`:3555 on 2023-01-07 03:36_

I think the backtick should come after `use` here

---

_Review comment by @harupy on `src/registry.rs`:3555 on 2023-01-07 03:37_

Thanks for the catch!

---

_@harupy reviewed on 2023-01-07 03:37_

---

_@harupy reviewed on 2023-01-07 03:37_

---

_Review comment by @harupy on `README.md`:929 on 2023-01-07 03:37_

```suggestion
| PT017 | AssertInExcept | Found assertion on exception ... in except block, use `pytest.raises()` instead |  |
```

---

_@harupy reviewed on 2023-01-07 03:38_

---

_Review comment by @harupy on `src/registry.rs`:3555 on 2023-01-07 03:38_

```suggestion
                    "Found assertion on exception {name} in except block, `use `pytest.raises()` \
```

---

_@harupy reviewed on 2023-01-07 03:38_

---

_Review comment by @harupy on `src/registry.rs`:3555 on 2023-01-07 03:38_

```suggestion
                    "Found assertion on exception {name} in except block, use `pytest.raises()` \
```

---

_Merged by @charliermarsh on 2023-01-07 03:55_

---

_Closed by @charliermarsh on 2023-01-07 03:55_

---
