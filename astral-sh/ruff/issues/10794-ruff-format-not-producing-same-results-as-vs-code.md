```yaml
number: 10794
title: "`ruff format` not producing same results as vs code extension"
type: issue
state: closed
author: alexdauenhauer
labels: []
assignees: []
created_at: 2024-04-05T21:12:07Z
updated_at: 2024-04-05T21:32:34Z
url: https://github.com/astral-sh/ruff/issues/10794
synced_at: 2026-01-12T15:54:50Z
```

# `ruff format` not producing same results as vs code extension

---

_@alexdauenhauer_

in my local repo I have these pyproject.toml settings
```toml
[tool.ruff]
line-length = 100
indent-width = 4
include = ["*.py", "*.pyi", "**/pyproject.toml", "*.ipynb"]

[tool.ruff.lint]
extend-select = ["D403"]

[tool.ruff.lint.per-file-ignores]
"*.ipynb" = ["E402", "F401"]

[tool.ruff.lint.isort]
force-single-line = true
lines-between-types = 1
order-by-type = false
```
and produces this when I run `ruff format --config=pyproject.toml`

https://github.com/astral-sh/ruff/assets/11903445/f05099dd-28fc-4ab9-a1e0-98a5abb8bcee


and also `ruff check --fix` says "All tests passed!"
whereas my vscode extension points to a `ruff.toml` which looks like this
```toml
line-length = 100
extend-select = ["D403"]
extend-include = ["*.ipynb"]

[isort]
force-single-line = true
lines-between-types = 1
order-by-type = false

[per-file-ignores]
"*.ipynb" = ["E402", "F401"]
```
and produces this result when I save


https://github.com/astral-sh/ruff/assets/11903445/079eca51-7343-4ae7-afe5-61f43c5edb45

The vs code extension results are the ones I want and I don't know how to achieve them


---

_Comment by @charliermarsh on 2024-04-05 21:14_

It looks like in the latter case, Ruff is attempting to sort the imports, whereas in the former it's not. Is that correct? You might have `organizeImports` enabled in VS Code, which will attempt to organize imports even if the rule isn't selected.

---

_Comment by @alexdauenhauer on 2024-04-05 21:24_

@charliermarsh yes correct, but how do I turn on `organizeImports` using the CLI?

---

_Comment by @charliermarsh on 2024-04-05 21:26_

Change:

```toml
[tool.ruff.lint]
extend-select = ["D403"]
```

To:

```toml
[tool.ruff.lint]
extend-select = ["D403", "I"]
```

---

_Comment by @alexdauenhauer on 2024-04-05 21:32_

I had to also do this
```sh
ruff check --fix --config=./pyproject.toml
ruff format --config=./pyproject.toml
```
but that worked! Thanks @charliermarsh !

---

_Closed by @alexdauenhauer on 2024-04-05 21:32_

---
