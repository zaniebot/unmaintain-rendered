```yaml
number: 9806
title: A parameter called parameters causes false positives for pydocstyle rules (D405, D406, D407, and D414)
type: issue
state: closed
author: owenlamont
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-02-03T11:54:58Z
updated_at: 2024-02-11T01:02:42Z
url: https://github.com/astral-sh/ruff/issues/9806
synced_at: 2026-01-10T11:09:52Z
```

# A parameter called parameters causes false positives for pydocstyle rules (D405, D406, D407, and D414)

---

_Issue opened by @owenlamont on 2024-02-03 11:54_

Ran into this bug where I'm working with a function with a parameter named _parameters_ and they caused false positive triggers (and incorrect automatic fixes) for the pydocstyle ruleset.

E.g.

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

The docstring for parameters (the parameter, not the section) is incorrectly interpreted as a misformatted section and if fix is enabled it is converted to a second redundant Parameters section which causes follow on errors like D414 section has no content...

I found I needed to disable rules D405 D406 D407 D414 to allow this to pass.

I ran:

```shell
ruff test.py --fix
```

Relevant section of my pyproject.toml is:

```toml
[tool.ruff.lint]
# See https://docs.astral.sh/ruff/rules/
select = ["A", "B", "C4", "D", "E", "F", "FURB", "I", "ISC", "Q", "RET", "T20", "UP"]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

[tool.ruff.lint.pydocstyle]
convention = "numpy"
```

I'm using ruff 0.2.0

---

_Label `bug` added by @charliermarsh on 2024-02-03 14:38_

---

_Label `docstring` added by @charliermarsh on 2024-02-03 14:38_

---

_Comment by @mikeleppane on 2024-02-07 09:52_

If you add type annotation for the parameters arg then rules D405, D406, D407, D414 should not trigger: 
```python  
def some_function(parameters: list[str]):
    """A function with a parameters parameter

    Parameters
    ----------

    parameters: list[str]
        A list of string parameters
    """
    ...
```
I have fixed this issue but now I am starting to wonder whether we should also warn about a missing type definition. My understanding is that this example violates [Numpy style guide](https://numpydoc.readthedocs.io/en/latest/format.html#parameters) if you read it very "strictly" .
What do you think @charliermarsh?  

---

_Comment by @tjkuson on 2024-02-07 10:50_

> I have fixed this issue but now I am starting to wonder whether we should also warn about a missing type definition.

I don't think this would be a particularly useful rule, as documenting types in a docstring is redundant if you use type annotations in the actual signature (documentation generators can read signatures). I can only think it would be useful in a codebase where Python type annotations are taboo for whatever reason...

---

_Comment by @mikeleppane on 2024-02-07 11:01_

Yes, it is kind of duplicate/redundant but nevertheless using it will fix the issue.

---

_Comment by @charliermarsh on 2024-02-08 02:52_

I think it would be best to fix the issue (as described) and not warn on missing type annotations -- at minimum, the rule should work without these false positives when type annotations are omitted.

(Also: thank you @mikeleppane for so many useful fixes!)

---

_Comment by @charliermarsh on 2024-02-11 00:09_

This was fixed in https://github.com/astral-sh/ruff/pull/9894.

---

_Closed by @charliermarsh on 2024-02-11 00:09_

---

_Comment by @owenlamont on 2024-02-11 01:02_

Thanks so much for resolving this so quickly!

---
