```yaml
number: 1838
title: Document that extend-select wins over more specific ignore
type: issue
state: closed
author: jankatins
labels: []
assignees: []
created_at: 2023-01-12T22:56:59Z
updated_at: 2024-11-22T01:03:49Z
url: https://github.com/astral-sh/ruff/issues/1838
synced_at: 2026-01-10T11:09:44Z
```

# Document that extend-select wins over more specific ignore

---

_Issue opened by @jankatins on 2023-01-12 22:56_

I've the following `example.py` file:

```
print("AA"
      + "BB")
```

My pyproject.toml specifies in the ruff section:

```
[tool.ruff]
extend-select = ["B0", "C4", "ICN", "ISC", "N", "RUF", "SIM"]
ignore = [
    # Explicitly concatenated string should be implicitly concatenated
    "ISC003",
]

```

Running ruff on it, results in this:

```
λ  ruff example.py
example.py:1:7: ISC003 Explicitly concatenated string should be implicitly concatenated
Found 1 error(s).
```

Using 
```
[tool.ruff]
extend-select = ["B0", "C4", "ICN", "ISC", "N", "RUF", "SIM"]
extend-ignore = [
    # Explicitly concatenated string should be implicitly concatenated
    "ISC003",
]
```

works as I wanted it to (no error), but I found that surprising, that the computation rules seems to be

1. Figure out what should be selected by looking at ignore and select settings, letting the more specific rules win
2. Add the extend-* rules and compute again 

I would have expected:
1. Use ignore/select and add the rules from extend-*
2. Compute the selected items by letting the more specific rules win 

Is this intended?

```
λ ruff --version
ruff 0.0.215
```

---

_Comment by @charliermarsh on 2023-01-12 22:59_

It is intended and it's a little different than the way these work in Flake8. The main reason being that we support extending `pyproject.toml` files, and without these semantics, it can be impossible for child configurations to override their parents, if the parent ignores and selects a specific prefix code.

I typically recommend using `select` over `extend-select` (and same for `ignore`) unless you're using that `pyproject.toml` extension. So in this case, I'd recommend:

```toml
[tool.ruff]
select = ["E", "F", "B0", "C4", "ICN", "ISC", "N", "RUF", "SIM"]
ignore = [
    # Explicitly concatenated string should be implicitly concatenated
    "ISC003",
]
```

(I agree we could improve the docs here.)


---

_Comment by @jankatins on 2023-01-13 00:10_

My reasoning was that I wanted to get any new defaults enabled and therefore liked the extend-select version. This feels a bit like there needs an overwrite-extend-ignore and so on :-)

In any case: a proposal for a doc extension for the current state is in PR https://github.com/charliermarsh/ruff/pull/1839

---

_Closed by @charliermarsh on 2023-01-13 00:44_

---

_Comment by @IliasAarab on 2024-11-21 14:07_

Trying to run the same initial example as @jankatins but I do not get the initial error anymore:

`example.py`:
```python
print("AA"
      + "BB")
```

```bash
[tool.ruff.lint]
extend-select = ["B0", "C4", "ICN", "ISC", "N", "RUF", "SIM"]
ignore = [
    # Explicitly concatenated string should be implicitly concatenated
    "ISC003",
]
```
```bash
ruff check example.py
All checks passed!
```

Has the behaviour been modified? 


---

_Comment by @jankatins on 2024-11-22 01:03_

I think yes: https://github.com/astral-sh/ruff/pull/2312

---
