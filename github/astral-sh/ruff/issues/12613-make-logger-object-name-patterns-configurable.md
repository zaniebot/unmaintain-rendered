---
number: 12613
title: Make logger object name patterns configurable
type: issue
state: open
author: lpulley
labels:
  - type-inference
  - help wanted
assignees: []
created_at: 2024-08-01T15:35:26Z
updated_at: 2025-09-16T19:30:29Z
url: https://github.com/astral-sh/ruff/issues/12613
synced_at: 2026-01-07T13:12:15-06:00
---

# Make logger object name patterns configurable

---

_Issue opened by @lpulley on 2024-08-01 15:35_

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

Currently, Ruff uses some hard-coded name "patterns" to determine whether a module-local object might be a logger (for the purposes of e.g. `flake8-logging-format` G rules).

https://github.com/astral-sh/ruff/blob/fc16d8d04d86aa94a8aac14bbb87089b64d443a1/crates/ruff_python_semantic/src/analyze/logging.rs#L60-L74

We've found that we have some loggers called `_LOG` which aren't matched, and I was disappointed to find that these aren't configurable. Maybe this should be a setting list of regular expressions or globs or something like that?

---

_Comment by @lpulley on 2024-08-01 15:43_

I suppose, at the very least, it would be nice to add `starts_with("_log")` and `starts_with("_LOG")`, but that seems like a band-aid.

---

_Referenced in [astral-sh/ruff#11854](../../astral-sh/ruff/issues/11854.md) on 2024-08-01 21:16_

---

_Label `type-inference` added by @MichaReiser on 2024-08-01 21:16_

---

_Comment by @MichaReiser on 2024-08-01 21:17_

This makes sense to me, at least short-term. The long term goal is to use type inference to detect if a variable is a logger

---

_Label `help wanted` added by @MichaReiser on 2024-08-01 21:17_

---

_Referenced in [astral-sh/ruff#20436](../../astral-sh/ruff/issues/20436.md) on 2025-09-16 17:08_

---

_Comment by @spaceone on 2025-09-16 19:30_

from duplicate: https://github.com/astral-sh/ruff/issues/20436

with config:
```toml
[tool.ruff.lint]
logger-objects = ['foo.get_it', 'AUTH']
```

The following use cases exists:
```python
from foo import get_it
get_it.info('foo %s' % (1,))   # detected

blah = get_it()
blah.info('foo %s' % (1,))  # not detected
get_it().info('foo %s' % (1,))  # not detected

blub = get_it
blub.info('foo %s' % (1,))  # also not detected

AUTH.info('foo %s' % (1,))  # also not detected
```

So, I would like logger-objects to be the result of a function/method call.
Furthermore I would like to specify the log variable names, our loggers are called `AUTH`, `NETWORK`, `ACL`, etc. and therefor don't match anything like `.*log.*`.
If I just put these names into logger-objects, they are ignored.

---
