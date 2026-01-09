---
number: 18199
title: ruff format does not support ignore despite explicit instructions to do so
type: issue
state: closed
author: utdrmac
labels:
  - configuration
  - help wanted
assignees: []
created_at: 2025-05-19T16:07:32Z
updated_at: 2025-05-20T14:26:48Z
url: https://github.com/astral-sh/ruff/issues/18199
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff format does not support ignore despite explicit instructions to do so

---

_Issue opened by @utdrmac on 2025-05-19 16:07_

### Summary

```
$ ruff --version
ruff 0.11.10

$ ruff format --check .build/mytool.py
warning: The following rule may cause conflicts when used with the formatter: `COM812`. To avoid unexpected behavior, we recommend disabling this rule, either by removing it from the `select` or `extend-select` configuration, or adding it to the `ignore` configuration.
```

The instructions tell me to add COM812 to the 'ignore' configuration. Sure thing:

```
$ cat ruff.toml
[lint]
select = ["ALL"]
ignore = ["C901", "D100", "D103", "D203", "D211", "D212"]

[format]
quote-style = "double"

$ ruff format --check .build/mytool.py
ruff failed
  Cause: Failed to load configuration `/path/ruff.toml`
  Cause: Failed to parse /path/ruff.toml
  Cause: TOML parse error at line 3, column 1
  |
3 | ignore = ["COM812"]
  | ^^^^^^
unknown field `ignore`, expected one of `exclude`, `preview`, `indent-style`, `quote-style`, `skip-magic-trailing-comma`, `line-ending`, `docstring-code-format`, `docstring-code-line-length`
```

Extremely contradictory instructions. Formatter says "do this", I did it, then formatter complains. I'm at a loss.

Please either A) update the warning output to provide proper instructions, or B) add ignore support to ruff.toml for `[format]` section to match instructions. Because I'm running 'ruff format' it does not make sense to add to the ignore section of [lint]. If I was running 'ruff check', sure, it makes sense (sorta) to add to [lint] section, but I'm running format option, so naturally, it should be added to the [format] section.

### Version

0.11.10

---

_Comment by @MichaReiser on 2025-05-19 16:10_

Oh, you have to add it to the `lint.ignore` section. But I can see how this is confusing. We can improve the message by using the full option name.

---

_Label `configuration` added by @MichaReiser on 2025-05-19 16:10_

---

_Label `help wanted` added by @MichaReiser on 2025-05-19 16:10_

---

_Comment by @CodeMan62 on 2025-05-19 19:04_

will investigate it and create a PR for this 

---

_Assigned to @CodeMan62 by @ntBre on 2025-05-19 20:32_

---

_Referenced in [astral-sh/ruff#18217](../../astral-sh/ruff/pulls/18217.md) on 2025-05-20 12:14_

---

_Closed by @MichaReiser on 2025-05-20 14:26_

---
