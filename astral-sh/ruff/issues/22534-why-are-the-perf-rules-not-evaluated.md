```yaml
number: 22534
title: Why are the PERF rules not evaluated?
type: issue
state: open
author: mosc9575
labels:
  - question
assignees: []
created_at: 2026-01-12T15:50:21Z
updated_at: 2026-01-12T15:52:33Z
url: https://github.com/astral-sh/ruff/issues/22534
synced_at: 2026-01-12T16:13:37Z
```

# Why are the PERF rules not evaluated?

---

_@mosc9575_

### Question

I want to enable the rules implemented by PERF and I am not able to get the expected warnings and errors.

Here is a minimal example code.
```python
d = {letter: i for i, letter in enumerate(list('abc'))}
for k, v in d.items():
    k = 1
```
and my expectation is that rule "PERF102" would suggest to use `d.keys()` instead of `d.items()`.

The setup in the `pyproject.toml` looks like 

```
[tool.ruff]
line-length = 100
target-version = "py312"

[tool.ruff.format]
docstring-code-format = true

[tool.ruff.lint]
exclude = [".git", ".ipynb_checkpoints"]
per-file-ignores = {"__init__.py" = ["F401", "F403"]}
select = ["A", "B", "C90", "COM", "E", "F", "I", "NPY", "PERF", "RUF", "TID", "UP", "W"]
ignore = ["B905", "COM812"]
mccabe.max-complexity=13

[tool.ruff.lint.pycodestyle]
max-line-length = 125
max-doc-length = 125
```

See also the [playgorund example](https://play.ruff.rs/6c61e315-5677-4979-bdb5-dfafd89d175b).

Why is the PERF rule not applied?

### Version

0.14.11

---

_Label `question` added by @mosc9575 on 2026-01-12 15:50_

---
