```yaml
number: 22534
title: Why are the PERF rules not evaluated?
type: issue
state: closed
author: mosc9575
labels:
  - question
assignees: []
created_at: 2026-01-12T15:50:21Z
updated_at: 2026-01-13T07:55:17Z
url: https://github.com/astral-sh/ruff/issues/22534
synced_at: 2026-01-13T08:23:00Z
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

_Comment by @ntBre on 2026-01-12 17:02_

Thanks for the report! I agree, this looks like a bug. I can't immediately tell from the rule implementation why it's not firing. At first I thought we failed to identify the comprehension as a dict, but this case triggers the rule:

```py
d = {letter: i for i, letter in enumerate(list('abc'))}
for k, v in d.items():
    print(k)
```

so it seems to be something with the assignment in the loop.

---

_Label `question` removed by @ntBre on 2026-01-12 17:02_

---

_Label `bug` added by @ntBre on 2026-01-12 17:02_

---

_Label `rule` added by @MichaReiser on 2026-01-12 17:18_

---

_Comment by @dylwil3 on 2026-01-12 18:52_

The rule does not trigger when neither the key nor value is actually used. The assignment `k = 1` in your example does not actually use the binding created in `for k, v in d.items()`, it just shadows the name.

When neither key nor value is used, it's not clear what we should be suggesting to use instead - `dict.keys()` doesn't seem quite right either.

Note that [redefined-loop-name (PLW2901)](https://docs.astral.sh/ruff/rules/redefined-loop-name/#redefined-loop-name-plw2901) will trigger in your example since the `k` is redefined before use.

---

_Label `bug` removed by @MichaReiser on 2026-01-12 18:54_

---

_Label `rule` removed by @MichaReiser on 2026-01-12 18:54_

---

_Label `question` added by @MichaReiser on 2026-01-12 18:54_

---

_Comment by @MichaReiser on 2026-01-12 18:55_

[Playground](https://play.ruff.rs/6fe10380-245f-459d-8a64-edbb8da2a8ed) showing that it works when using `k` in the loop body

---

_Comment by @mosc9575 on 2026-01-13 07:55_

Thanks for your fast and kind reply. My question is now solved and I will close this issue.

---

_Closed by @mosc9575 on 2026-01-13 07:55_

---
