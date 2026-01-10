```yaml
number: 12403
title: RUF013 does not check usage of implicit None in class constructors
type: issue
state: closed
author: mikeweltevrede
labels:
  - needs-info
assignees: []
created_at: 2024-07-19T07:18:13Z
updated_at: 2024-07-22T14:05:47Z
url: https://github.com/astral-sh/ruff/issues/12403
synced_at: 2026-01-10T11:09:54Z
```

# RUF013 does not check usage of implicit None in class constructors

---

_Issue opened by @mikeweltevrede on 2024-07-19 07:18_

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

I noticed that rule [RUF013](https://docs.astral.sh/ruff/rules/implicit-optional/) did not check usage of implicit optionals in the class constructors `__init__()`. I would expect to see `RUF013 PEP 484 prohibits implicit 'Optional'`, but instead, it passes the checks.

# List of keywords you searched for before creating this issue
- RUF013
- Optional

# A minimal code snippet that reproduces the bug
```python
class MyClass:
    def __init__(self, a: int = None): ...
```

# The command you invoked
- `ruff check --select RUF013`
- `pre-commit run --all-files`

# The current Ruff settings
```toml
[tool.ruff]
line-length = 120
target-version = "py39"

[tool.ruff.lint]
extend-select = [
    ...,
    "RUF",
    ...,
]
```

# The current Ruff version
`0.5.3` but also tried with earlier versions `0.3.0` and `0.4.0`.

---

_Comment by @charliermarsh on 2024-07-19 13:30_

This actually is flagged for me in the playground: https://play.ruff.rs/ed0f6c85-acdf-4e74-a3de-c17deb79f0ef

---

_Comment by @charliermarsh on 2024-07-19 13:31_

And on the command-line too:

```
❯ ruff check foo.py --select RUF --isolated
foo.py:2:27: RUF013 PEP 484 prohibits implicit `Optional`
  |
1 | class MyClass:
2 |     def __init__(self, a: int = None): ...
  |                           ^^^ RUF013
  |
  = help: Convert to `Optional[T]`
```

Maybe something else going on? Is that the exact MRE you used?

---

_Label `needs-info` added by @charliermarsh on 2024-07-19 13:31_

---

_Comment by @mikeweltevrede on 2024-07-19 14:30_

Thanks @charliermarsh, I will go through our situation with a fine-tooth comb and see if something else might be interfering with this. If it works well in the ruff playground for you, then it must be something on our side that I am not seeing.

---

_Comment by @charliermarsh on 2024-07-19 14:46_

We have some logic in there to detect annotations that implicitly allow `None` (like `Any`). If you find a concrete example, I might be able to map it back to that logic (and figure out whether it's intended or not).

---

_Comment by @mikeweltevrede on 2024-07-22 14:05_

Hi @charliermarsh, I tried to reproduce again but could not. Must have been some other setting in our project that got updated. Thanks for your quick response.

---

_Closed by @mikeweltevrede on 2024-07-22 14:05_

---
