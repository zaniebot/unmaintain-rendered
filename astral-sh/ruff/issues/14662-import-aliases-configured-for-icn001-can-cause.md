---
number: 14662
title: Import aliases configured for ICN001 can cause syntax errors
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-11-28T19:26:14Z
updated_at: 2024-12-03T04:41:48Z
url: https://github.com/astral-sh/ruff/issues/14662
synced_at: 2026-01-10T01:22:55Z
---

# Import aliases configured for ICN001 can cause syntax errors

---

_Issue opened by @dscorbett on 2024-11-28 19:26_

Ruff 0.8.0 does not validate the aliases in [`lint.flake8-import-conventions.extend-aliases`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_extend-aliases) for [`unconventional-import-alias` (ICN001)](https://docs.astral.sh/ruff/rules/unconventional-import-alias/).

```console
$ cat icn001.py
import sys

$ ruff check --select ICN001 --config 'lint.flake8-import-conventions.extend-aliases = {"sys" = "def"}' --isolated --unsafe-fixes --fix icn001.py

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `icn001.py`, the rule codes ICN001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

icn001.py:1:8: ICN001 `sys` should be imported as `def`
  |
1 | import sys
  |        ^^^ ICN001
  |
  = help: Alias `sys` to `def`

Found 1 error.
[*] 1 fixable with the --fix option.
```

The extra aliases should be validated like the ones in [`lint.flake8-import-conventions.aliases`](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases) (#14477).

```console
$ ruff check --select ICN001 --config 'lint.flake8-import-conventions.aliases = {"sys" = "def"}' --isolated --unsafe-fixes --fix icn001.py
error: invalid value 'lint.flake8-import-conventions.aliases = {"sys" = "def"}' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

Could not parse the supplied argument as a `ruff.toml` configuration option:

invalid value: string "def", expected a Python identifier
in `lint`

For more information, try '--help'.
```

There is still one problem with `lint.flake8-import-conventions.aliases`, and therefore also `extend-aliases`: they allow `__debug__` as an alias, but that is a syntax error.

```console
$ ruff check --select ICN001 --config 'lint.flake8-import-conventions.aliases = {"sys" = "__debug__"}' --isolated --unsafe-fixes --fix icn001.py
Found 1 error (1 fixed, 0 remaining).

$ cat icn001.py
import sys as __debug__

$ python icn001.py
  File "icn001.py", line 1
    import sys as __debug__
    ^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: cannot assign to __debug__
```

---

_Comment by @AlexWaygood on 2024-11-28 20:35_

Cc. @dylwil3, if you're interested in this one!

---

_Label `bug` added by @AlexWaygood on 2024-11-28 20:36_

---

_Label `configuration` added by @AlexWaygood on 2024-11-28 20:36_

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-28 22:35_

---

_Referenced in [astral-sh/ruff#14745](../../astral-sh/ruff/pulls/14745.md) on 2024-12-03 00:08_

---

_Closed by @dylwil3 on 2024-12-03 04:41_

---

_Closed by @dylwil3 on 2024-12-03 04:41_

---
