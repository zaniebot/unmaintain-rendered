```yaml
number: 9894
title: "[`pydocstyle-D405`] Allow using `parameters` as a sub-section header"
type: pull_request
state: merged
author: mikeleppane
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: fix(D405)/allow_using_section_name_as_sub_section_header
created_at: 2024-02-08T12:55:48Z
updated_at: 2024-02-09T02:54:39Z
url: https://github.com/astral-sh/ruff/pull/9894
synced_at: 2026-01-12T15:55:30Z
```

# [`pydocstyle-D405`] Allow using `parameters` as a sub-section header

---

_@mikeleppane_

## Summary

This review contains a fix for [D405](https://docs.astral.sh/ruff/rules/capitalize-section-name/) (capitalize-section-name)
The problem is that Ruff considers the sub-section header as a normal section if it has the same name as some section name. For instance, a function/method has an argument named "parameters".  This only applies if you use Numpy style docstring.

See: [ISSUE](https://github.com/astral-sh/ruff/issues/9806)

The following will not raise D405 after the fix:
```python  
def some_function(parameters: list[str]):
    """A function with a parameters parameter

    Parameters
    ----------

    parameters:
        A list of string parameters
    """
    ...
```


## Test Plan

```bash
cargo test
```


---

_Renamed from "[`pydocstyle-D405`] Allow using *parameters* as a sub-section header" to "[`pydocstyle-D405`] Allow using `parameters` as a sub-section header" by @mikeleppane on 2024-02-08 12:56_

---

_Comment by @github-actions[bot] on 2024-02-08 13:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-08 17:39_

---

_Label `bug` added by @charliermarsh on 2024-02-08 17:39_

---

_Label `docstring` added by @charliermarsh on 2024-02-08 17:39_

---

_Comment by @mikeleppane on 2024-02-08 21:17_

Note: check that the condition still holds if an arg name is different than Parameters but some other section name e.g returns.

---

_Comment by @charliermarsh on 2024-02-08 21:18_

@mikeleppane - Can you post an example?

---

_Comment by @mikeleppane on 2024-02-08 21:23_

Sorry, I forgot to add a test case for this.

```python

def foo(returns: int):
   """
   Parameters
   -‐-----------------
   returns:
       some value

   """

---

_Comment by @charliermarsh on 2024-02-08 21:28_

Ahh ok yes, the change I made here doesn't cover that case.

---

_Comment by @charliermarsh on 2024-02-08 21:30_

I have an idea for it, thanks.

---

_Merged by @charliermarsh on 2024-02-09 02:54_

---

_Closed by @charliermarsh on 2024-02-09 02:54_

---

_Comment by @charliermarsh on 2024-02-09 02:54_

Thanks as always @mikeleppane!

---
