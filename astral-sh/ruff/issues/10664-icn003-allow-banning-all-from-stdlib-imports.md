```yaml
number: 10664
title: "`ICN003`: Allow banning all `from` stdlib imports"
type: issue
state: open
author: Grub4K
labels:
  - rule
  - configuration
assignees: []
created_at: 2024-03-29T22:05:24Z
updated_at: 2025-12-25T17:06:47Z
url: https://github.com/astral-sh/ruff/issues/10664
synced_at: 2026-01-12T15:54:50Z
```

# `ICN003`: Allow banning all `from` stdlib imports

---

_@Grub4K_

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
Our project decided to no longer allow stdlib imports with `from` due to potentially clashing naming causing confusion.
We are currently resorting to including every stdlib in the `flake8-import-conventions` `banned-from` setting:
```toml
[tool.ruff.lint.flake8-import-conventions]
banned-from = ["abc", "aifc", "antigravity", "argparse", "array", "ast", "asynchat", "asyncio", "asyncore", "atexit", "..."]
```
However this is very cumbersome. Is there a possiblity of referencing all stdlib modules? A shortcut like this could perhaps also be used for different importing settings.

I've searched for `ICN003`, `banned-from` and `banned-import-from`, but could not find anything related.

---

_Label `rule` added by @charliermarsh on 2024-03-30 00:40_

---

_Comment by @dhruvmanila on 2024-04-01 08:57_

Thanks for the clear write-up!

I would rather go with adding this as a separate rule. @charliermarsh Is this why you marked this with "rule" label?

---

_Comment by @zanieb on 2024-04-02 02:58_

Why a separate rule if it's the same idea?

cc @AlexWaygood 

---

_Label `configuration` added by @zanieb on 2024-04-02 02:58_

---

_Comment by @charliermarsh on 2024-04-02 03:37_

> Is this why you marked this with "rule" label?

(No, the "rule" label can represent either a new rule or a change to an existing rule.)

---

_Comment by @dhruvmanila on 2024-04-05 10:00_

> Why a separate rule if it's the same idea?

This might be a stretch but it could lead to name collisions. So, we could either have "stdlib" as a possible value or maybe provide a new config option for that rule (`banned-from-stdlib = true`).

---

_Comment by @carljm on 2024-04-05 14:49_

I'm curious why the stdlib would be special in terms of "potentially clashing naming causing confusion." What about other third-party libraries? Should the option here really be `banned-from-non-first-party`?

---

_Comment by @Grub4K on 2024-04-05 18:45_

Maybe `banned-from-non-first-party` could have `"stdlib"` as a sentinel to signify only stdlib for that case? Having it as a boolean option alone could mean not having enough granular control over what to ban?

---

_Comment by @Grub4K on 2025-12-25 17:06_

> This might be a stretch but it could lead to name collisions

This could be avoided if we use something that is invalid as a name, like `:all:`. pip does this for `--no-binary` for example

---
