```yaml
number: 3683
title: Setting for a rule enables it implicitly
type: issue
state: closed
author: kytta
labels:
  - bug
assignees: []
created_at: 2023-03-23T11:20:14Z
updated_at: 2023-03-23T17:06:23Z
url: https://github.com/astral-sh/ruff/issues/3683
synced_at: 2026-01-12T15:54:43Z
```

# Setting for a rule enables it implicitly

---

_@kytta_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have a big Python project that I am migrating to use Ruff. For now, I disable some of the rules in order to enable them later (I do not want to break my CI just yet). However, I already defined some settings for the rules I will use in the future.

Here's a very minimal example:

```toml
# pyproject.toml

[tool.ruff]
extend-select = [
    # TODO: fix docs in a separate issue
    # "D",  # pydocstyle
    "PT",  # flake8-pytest-style
]

fixable = [
    "PT001",  # pytest fixture with parens
    "PT006",  # tuple in pytest parametrize
]

[tool.ruff.pydocstyle]
convention = "pep257"
```

```py
# main.py

def my_func(arg):
   '''Return something
   param docs are not provided'''


   return arg+1
```

## What I expect

I expect that when I run Ruff, it will only care about the enabled rules (the default `E` and `F` as well as the `PT`). `D` is not selected, so Ruff is not bothering to fix it.

## Actual behaviour
Ruff detects (and attempts to fix) the `D` issues:

```bash
$ ruff . --format=grouped --show-fixes   
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
myproject/main.py:
  2:4  D415 [*] First line should end with a period, question mark, or exclamation point

Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

## Workaround?

I don't know if it's expected behaviour or not. In any case, if I also comment the `[tool.ruff.pydocstyle]` out, the ruleset gets ignored properly

---

Ruff version is 0.0.258

---

_Comment by @JonathanPlasse on 2023-03-23 11:57_

`convention` is used to select which pydocstyle use and enable the corresponding `D` rules.
https://beta.ruff.rs/docs/settings/#convention
Why did you add it to your configuration?

---

_Comment by @kytta on 2023-03-23 13:13_

> Why did you add it to your configuration?

I generally want to enforce it, but not at this point in time, since the repo is too "dirty". That's why I temporarily disabled the rule inside `select`.

> [...] and enable the corresponding D rules.

I see, I guess it's desired behaviour, then. Thanks!

---

_Closed by @kytta on 2023-03-23 13:13_

---

_Comment by @charliermarsh on 2023-03-23 13:48_

I thought enabling `D` was required, this seems like a bug, lemme look into it.

---

_Reopened by @charliermarsh on 2023-03-23 13:48_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-23 13:48_

---

_Comment by @charliermarsh on 2023-03-23 13:58_

Sorry about that, this is a confirmed regression in the latest release. You can downgrade to 0.0.257 for now, but we'll ship a fix soon.

---

_Label `bug` added by @charliermarsh on 2023-03-23 13:58_

---

_Comment by @charliermarsh on 2023-03-23 17:06_

Should be fixed by https://github.com/charliermarsh/ruff/pull/3685, which'll go out later today.

---

_Closed by @charliermarsh on 2023-03-23 17:06_

---
