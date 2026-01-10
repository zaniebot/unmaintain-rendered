```yaml
number: 8217
title: "Docs: README.md `tool.ruff.lint` fault"
type: issue
state: closed
author: thernstig
labels: []
assignees: []
created_at: 2023-10-25T13:44:16Z
updated_at: 2023-10-25T14:42:07Z
url: https://github.com/astral-sh/ruff/issues/8217
synced_at: 2026-01-10T11:09:50Z
```

# Docs: README.md `tool.ruff.lint` fault

---

_Issue opened by @thernstig on 2023-10-25 13:44_

```toml
[tool.ruff.lint]
# 1. Enable flake8-bugbear (`B`) rules, in addition to the defaults.
select = ["E", "F", "B"]
```

The comment says it only enables flake8-bugbear in addition to defaults, but the default is this according to the same page:

```toml
[tool.ruff.lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`)  codes by default.
# Unlike Flake8, Ruff doesn't enable pycodestyle warnings (`W`) or
# McCabe complexity (`C901`) by default.
select = ["E4", "E7", "E9", "F"]
ignore = []
```

So the above is enabling all `E` rules in addition to the flake8-bugbear rules.

**Solution**

Change to:

```toml
[tool.ruff.lint]
# 1. Enable flake8-bugbear (`B`) rules and all (`E`) rules, in addition to the defaults.
select = ["E", "F", "B"]
```

---

_Closed by @zanieb on 2023-10-25 14:42_

---
