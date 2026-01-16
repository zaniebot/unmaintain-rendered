```yaml
number: 22620
title: "Do not autofix F541 `f-string without any placeholders` / mark as risky"
type: issue
state: open
author: k0pernikus
labels: []
assignees: []
created_at: 2026-01-16T12:36:06Z
updated_at: 2026-01-16T12:45:40Z
url: https://github.com/astral-sh/ruff/issues/22620
synced_at: 2026-01-16T12:56:19Z
```

# Do not autofix F541 `f-string without any placeholders` / mark as risky

---

_@k0pernikus_

### Summary

Given this code:

```python
class Greeter:
    def hello(self, name: str) -> str:
        return f"Hello name!"
```

ruff will correctly report:

```
F541 [*] f-string without any placeholders
 --> utils\src\utils\greeting.py:3:16
  |
1 | class Greeter:
2 |     def hello(self, name: str) -> str:
3 |         return f"Hello name!"
  |                ^^^^^^^^^^^^^^
  |
```

Yet its autofix hides the bug the developer made:

```
help: Remove extraneous `f` prefix

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

It doesn't really help that it does not produce an output:

```
ruff check --fix
Found 1 error (1 fixed, 0 remaining).
```

File will now contain:

```
class Greeter:
    def hello(self, name: str) -> str:
        return "Hello name!"
```

----

This is my `ruff.toml`:

```
target-version = "py314"
line-length = 88
indent-width = 4

[lint]
select = [
    "E",
    "F",
    "UP",
    "B",
    "SIM",
    "I",
    "W",
]
ignore = []

[lint.isort]
combine-as-imports = true
lines-after-imports = 2

[format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
```

---

Small unit test for the Greeter:

```python
def test_hello_world():
    greeter = Greeter()
    msg = greeter.hello("User")

    assert msg == "Hello User!"
```

The code as was intended by the devloper:

```python
class Greeter:
    def hello(self, name: str) -> str:
        return f"Hello {name}!"
```

---

This might also relate to how [ARG001](https://docs.astral.sh/ruff/rules/unused-function-argument/), [ARG002](https://docs.astral.sh/ruff/rules/unused-method-argument/) are not part of the recommended rule set, but `F` is:


> For example, a configuration that enables some of the most popular rules (without being too pedantic) might look like the following:
> 
> ```
> [lint]
> select = [
>     # pycodestyle
>     "E",
>     # Pyflakes
>     "F",
>     # pyupgrade
>     "UP",
>     # flake8-bugbear
>     "B",
>     # flake8-simplify
>     "SIM",
>     # isort
>     "I",
> ]
> ```

-- [ruff on rule selection](https://docs.astral.sh/ruff/linter/#rule-selection)





---

_Comment by @k0pernikus on 2026-01-16 12:43_

I added [`extend-unsafe-fixes`](https://docs.astral.sh/ruff/settings/#lint_extend-unsafe-fixes) for that rule in my project's `ruff.toml`:

```toml
[lint]
select = [
    "E",
    "F",
    "UP",
    "B",
    "SIM",
    "I",
    "W",
]

ignore = []

extend-unsafe-fixes = ["F541"]
```

That works for me:

```shell
ruff check --fix 
F541 f-string without any placeholders
 --> utils\src\utils\greeting.py:3:16
  |
1 | class Greeter:
2 |     def hello(self, name: str) -> str:
3 |         return f"Hello name!"
  |                ^^^^^^^^^^^^^^
  |
help: Remove extraneous `f` prefix

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

It is also really great that you have also included the `--unsafe-fixes`-flag!


---
