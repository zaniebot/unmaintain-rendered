```yaml
number: 13329
title: "[BUG] `ruff` does not correctly lint positional only arguments"
type: issue
state: open
author: yarnabrina
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-09-11T20:11:59Z
updated_at: 2024-10-24T14:33:08Z
url: https://github.com/astral-sh/ruff/issues/13329
synced_at: 2026-01-10T11:09:55Z
```

# [BUG] `ruff` does not correctly lint positional only arguments

---

_Issue opened by @yarnabrina on 2024-09-11 20:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Consider the following snippet:

```python
# contents of ruff_bug_report.py
def add(x: int, y: int, /) -> int:
    return x + y


z = add(x=1, y=1)

```

This code fails because of positional only arguments.

```zsh
❯ python ruff_bug_report.py
Traceback (most recent call last):
  File "/home/anirban/sktime-fork/ruff_bug_report.py", line 5, in <module>
    z = add(x=1, y=1)
TypeError: add() got some positional-only arguments passed as keyword arguments: 'x, y'
```

However, `ruff` check passes.

```zsh
❯ ruff check ruff_bug_report.py
All checks passed!
```

This check is part of `pylint`, ref. [E3102](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/positional-only-arguments-expected.html).

I am using `ruff==0.6.4`, and observed this on multiple python versions (`3.10`, `3.11`) on different operating systems (`macOS`, `Windows`).

---

_Label `rule` added by @MichaReiser on 2024-09-11 20:42_

---

_Label `multifile-analysis` added by @MichaReiser on 2024-09-11 20:42_

---

_Comment by @MichaReiser on 2024-09-11 20:42_

Thanks for opening this issue. 

I quickly checked, and Ruff doesn't yet implement the rule mentioned. This is most likely because it would require multifile analysis, or the rule would only flag calls to functions defined in the same file, which isn't that useful.

---

_Comment by @yarnabrina on 2024-09-11 21:13_

Oh, I wasn't aware of lack of multi file analysis. Do you mean that if I incorrectly use a function/class/etc. defuned in module A in a different module B, as of now ruff is unable to flag those cases?

---

_Comment by @MichaReiser on 2024-09-12 01:58_

> Oh, I wasn't aware of lack of multi file analysis. Do you mean that if I incorrectly use a function/class/etc. defuned in module A in a different module B, as of now ruff is unable to flag those cases?

That's correct. Ruff can't resolve functions across files. That's also the reason why many rules that require that kind of information aren't implemented today. We're working towards supporting this but it will take some more time until it becomes available in ruff.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
