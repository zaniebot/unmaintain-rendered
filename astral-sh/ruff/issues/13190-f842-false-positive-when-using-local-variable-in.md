```yaml
number: 13190
title: F842 false positive when using local variable in list comprehension
type: issue
state: closed
author: billwallis
labels:
  - question
assignees: []
created_at: 2024-09-01T10:01:19Z
updated_at: 2024-09-02T06:11:14Z
url: https://github.com/astral-sh/ruff/issues/13190
synced_at: 2026-01-10T11:09:55Z
```

# F842 false positive when using local variable in list comprehension

---

_Issue opened by @billwallis on 2024-09-01 10:01_

> [!NOTE]
>
> Keywords searched for before raising this issue:
>
> - `F842`
> - `unused-annotation`
> - `false positive`
> - `list comprehension`

## Issue summary

Rule [F842 (`unused-annotation`)](https://docs.astral.sh/ruff/rules/unused-annotation/) is incorrectly flagging a type-annotated local variable as unused when the variable is used in a list comprehension.

For example, the snippet below:

```python
def main():
    var: int
    [print(var) for var in range(1)]
```

...will cause the following linting error:

```
...: F842 Local variable `var` is annotated but never used
  |
1 | def main():
2 |     var: int
  |     ^^^ F842
3 |     [print(var) for var in range(1)]
```

Note, however, that this does **_NOT_** get flagged when `var` is a global variable:

```python
var: int  # this is fine
[print(var) for var in range(1)]
```

---

## Configuration and invocation details

I use Ruff in pre-commit -- hooks config is:

```yaml
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.6.3
  hooks:
    - id: ruff
      args: ["--fix"]
    - id: ruff-format
```

...and Ruff was invoked both via `git commit` and `pre-commit run ruff --all-files`.

My `pyproject.toml` settings for Ruff are:

```toml
[tool.ruff]
line-length = 80
indent-width = 4
target-version = "py311"

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

[tool.ruff.lint]
select = ["F", "I", "N", "PL", "R", "RUF", "S", "UP", "W"]
ignore = []
fixable = ["ALL"]
unfixable = []
# Allow unused variables when underscore-prefixed
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

# https://github.com/astral-sh/ruff/issues/4368
[tool.ruff.lint.extend-per-file-ignores]
"tests/**/*.py" = [
    "S101",    #  Use of `assert` detected
    "PLR2004", #  Magic value used in comparison
]
```

Environment details:

- Operating system: `Windows 10`
- Python version: `3.11.5`
- Pre-commit version: `3.8.0`
- Ruff version: `0.6.3`

---

_Label `bug` added by @AlexWaygood on 2024-09-01 10:14_

---

_Comment by @dylwil3 on 2024-09-01 15:53_

Is this a bug? Isn't the `var` that appears in the comprehension in it's own local scope?

Like if I do:

```python
x = 3
[print(x) for x in ["a","b","c"]]
print(x)
```
I get the output:
```
a
b
c
3
```
So even if I know the first `x` is an int, there's no need for it to still be an `int` inside the comprehension.

And the global variable example is part of the rule's documented behavior: "Checks for local variables that are annotated but never used." (Presumably global variables are never checked in the 'used' rules since they may be imported - though this case is a little weird since I'm not sure people are declaring types of global variables without assigning them, then later importing those unbound variables... But in any event it is expected behavior for the rule, I think).

---

_Label `bug` removed by @AlexWaygood on 2024-09-01 16:06_

---

_Comment by @AlexWaygood on 2024-09-01 16:08_

Very good point @dylwil3! Apparently that's the [second time](https://github.com/astral-sh/ruff/pull/13177#discussion_r1739860392) I've managed to confuse myself about the scoping of comprehension variables this weekend...

@Bilbottom does that make sense to you?

---

_Label `question` added by @AlexWaygood on 2024-09-01 16:08_

---

_Comment by @billwallis on 2024-09-02 06:11_

Thanks @dylwil3 and @AlexWaygood for the quick replies 🤓 

I also didn’t think about scope... and I totally agree that the variable in the function scope doesn’t have to be the same type as the variable in the list comprehension scope 😋 

---

_Closed by @billwallis on 2024-09-02 06:11_

---
