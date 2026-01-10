```yaml
number: 3024
title: Imports are formatted with different max-line-length then fixed
type: issue
state: closed
author: Nicholas-Autio-Mitchell
labels:
  - question
assignees: []
created_at: 2023-02-19T03:50:32Z
updated_at: 2023-02-19T14:59:17Z
url: https://github.com/astral-sh/ruff/issues/3024
synced_at: 2026-01-10T11:09:45Z
```

# Imports are formatted with different max-line-length then fixed

---

_Issue opened by @Nicholas-Autio-Mitchell on 2023-02-19 03:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Just discovered `ruff` recently - amazing work - thanks!

I show the problem in the short video below. When running ruff (via vscode's `formatOnSave`), an import line of length >79 is formatted to break imports over several lines - then black/ruff settings of max line length 120 are respected, reformatting the imports. I run this 5 times in the video:

https://user-images.githubusercontent.com/12820901/219914833-90165e62-2ac5-43e1-b607-a7e5c653ac55.mov


* I am using the vscode plugin, shipping with ruff version `ruff 0.0.239`.
* `black, 23.1.0 (compiled: yes) -- Python (CPython) 3.9.16`
* The effect is not observable when plain ruff from the command line without the plugin.
* This could well be a conflict in configs (I'm new here:)


## Minimal example

These files reproduce the video example.

### `entrypoint.py`
```python
"""Imports with a line length over 79 are formatted by some rule(?), then reformatted by black/ruff max line length."""

from busy_module import ReallyLongClassName1, ReallyLongClassName2, ReallyLongClassName3, ReallyLongClassName4

# Just to hide "unused import" warning
print(ReallyLongClassName1)
print(ReallyLongClassName2)
print(ReallyLongClassName3)
print(ReallyLongClassName4)
```

### `busy_module.py`

```python
class ReallyLongClassName1: pass

class ReallyLongClassName2: pass

class ReallyLongClassName3: pass

class ReallyLongClassName4: pass
```

## `pyproject.yaml`

```toml
[tool.black]
line-length = 120
target-version = ["py39"]

[tool.ruff]
# Must match the Black formatter.
line-length = 120
target-version = "py39"

select = ["ALL"]
ignore = ["ANN101", "D202", "INP001", "TCH003", "I001"]

# Allow autofix for all enabled rules (when `--fix`) is provided.
fixable = ["A", "B", "C", "D", "E", "F"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[tool.ruff.mccabe]
# Unlike Flake8, default to a complexity level of 10.
max-complexity = 10
```

---

_Comment by @charliermarsh on 2023-02-19 12:12_

Hey thanks for the kind words!

My best guess is that you have isort enabled (since the style of formatting that's being applied to the imports matches isort's behavior IIRC, but doesn't match Ruff's).

Can you try disabling the isort extension in VS Code?


---

_Label `question` added by @charliermarsh on 2023-02-19 12:12_

---

_Comment by @Nicholas-Autio-Mitchell on 2023-02-19 14:54_

🤦  the `isort` plugin was sync'd and enabled globally - deactivating resolves the issue 🙌

@charliermarsh - one quick question - I saw in a thread somewhere that there is an `"ALL"` tag for the `select` config. Is there also one for `fixable`?  I can't find any documentation on this, but want to check the following is a valid starting point to build from, while learning about ruff:

```yaml
[tool.ruff]
select = ["ALL"]
fixable = ["ALL"]
```

---

_Comment by @charliermarsh on 2023-02-19 14:59_

Yeah that works.

So in that case, you'd start from `ALL`, then disable rules selectively as you go:

```toml
[tool.ruff]
select = ["ALL"]
ignore = [
  # Or whatever you want to turn off.
  "D100",
]

fixable = ["ALL"]
unfixable = [
  # Maybe you want to avoid autofixing `SIM` violations.
  # Note that any rules you include in `ignore` don't have to be
  # included here, since we won't even _try_ to fix those.
  "SIM",
]
```

The other approach is to explicitly build up the list of rules you _do_ care about, like:

```toml
select = [
  "E",
  "F",
  "SIM",  # flake8-simplify
  "B",  # flake8-bugbear
]
```

I tend to prefer the latter approach as it's more explicit but it's just a personal preference.


---

_Closed by @charliermarsh on 2023-02-19 14:59_

---
