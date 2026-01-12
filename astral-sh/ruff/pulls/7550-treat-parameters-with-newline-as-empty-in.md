```yaml
number: 7550
title: Treat parameters-with-newline as empty in function formatting
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/annotations
created_at: 2023-09-20T17:02:19Z
updated_at: 2023-09-20T20:20:23Z
url: https://github.com/astral-sh/ruff/pull/7550
synced_at: 2026-01-12T15:55:24Z
```

# Treat parameters-with-newline as empty in function formatting

---

_@charliermarsh_

## Summary

If a function has no parameters (and no comments within the parameters' `()`), we're supposed to wrap the return annotation _whenever_ it breaks. However, our `empty_parameters` test didn't properly account for the case in which the parameters include a newline (but no other content), like:

```python
def get_dashboards_hierarchy(
) -> Dict[Type['BaseDashboard'], List[Type['BaseDashboard']]]:
    """Get hierarchy of dashboards classes.

    Returns:
        Dict of dashboards classes.
    """
    dashboards_hierarchy = {}
```

This PR fixes that detection. Instead of lexing, it now checks if the parameters itself is empty (or if it contains comments).

Closes https://github.com/astral-sh/ruff/issues/7457.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-20 17:02_

---

_Review requested from @konstin by @charliermarsh on 2023-09-20 17:02_

---

_Label `formatter` added by @charliermarsh on 2023-09-20 17:02_

---

_@charliermarsh reviewed on 2023-09-20 17:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:185 on 2023-09-20 17:02_

I ditched the tokenizer in favor of `parameters.is_empty() && !comments.ranges().intersects(parameters.range())`.

---

_Comment by @github-actions[bot] on 2023-09-20 17:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:150 on 2023-09-20 19:38_

What's the reason for using comment.ranges instead of comments.has_comments(paramteters)?

I would prefer if we use comments as the source of truth to ensure it's in sync with what we format and don't start relying on multiple sources 

---

_@MichaReiser reviewed on 2023-09-20 19:38_

---

_@charliermarsh reviewed on 2023-09-20 19:48_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:150 on 2023-09-20 19:48_

Ah I just forgot.

---

_@charliermarsh reviewed on 2023-09-20 19:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:150 on 2023-09-20 19:49_

Ah I just forgot.

---

_@charliermarsh reviewed on 2023-09-20 19:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:150 on 2023-09-20 19:49_

Fixed.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-20 19:49_

---

_@MichaReiser approved on 2023-09-20 20:06_

---

_Merged by @charliermarsh on 2023-09-20 20:20_

---

_Closed by @charliermarsh on 2023-09-20 20:20_

---

_Branch deleted on 2023-09-20 20:20_

---
