```yaml
number: 13378
title: When set to parents (default) ban-relative-imports setting results in no errors despite parent relative imports being present
type: issue
state: closed
author: lgig
labels:
  - question
assignees: []
created_at: 2024-09-17T09:29:35Z
updated_at: 2024-09-18T06:58:43Z
url: https://github.com/astral-sh/ruff/issues/13378
synced_at: 2026-01-12T15:54:53Z
```

# When set to parents (default) ban-relative-imports setting results in no errors despite parent relative imports being present

---

_@lgig_

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
Hello,

I'm trying to configure ruff to ban parent relative imports as described in rule [TID252](https://docs.astral.sh/ruff/rules/relative-imports/) and [ban-relative-imports](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_ban-relative-imports) setting, but I can't seem to make it work. Am I missing something or could this be a bug?

I tried searching issues for `ban-relative-imports` and `parents`, but none seem related.

Directory structure:
```
__init__.py
a.py
one/
    __init__.py
    b.py
    two/
        __init__.py
        c.py
        d.py
```
`d.py`:
```
from ...a import A # Should be an error (fixable into `from a import A`?)
from ..b import B # Should be an error (fixable into `from one.B import B`?)
from .c import C # Should be ok

class D(A, B, C):
    ...
```
`c.py`:
```
class C:
    ...
```
`b.py`:
```
class B:
    ...
```
`a.py`:
```
class A:
    ...
```
My command (launched from project root):
```
$ ruff check
All checks passed!
```
While I expected two errors from `d.py`.\
(obvious errors added on purpose inside `d.py` are recognized so the file is being checked)

My settings (`ruff.toml`):
```
[lint.flake8-tidy-imports]
ban-relative-imports = "parents"
```
These seem optional as `"parents"` is the default value. The setting seems to be recognized:
```
$ ruff check --show-settings | grep ban_relative_imports
linter.flake8_tidy_imports.ban_relative_imports = "parents"
```
Version:
```
$ ruff --version
ruff 0.6.5
```

Thank you for your time.


---

_Comment by @charliermarsh on 2024-09-17 14:10_

Did you enable the rule, e.g., with `extend-select = ["TID252"]`? It won't run by default, even if that setting is provided.

---

_Label `question` added by @charliermarsh on 2024-09-17 14:10_

---

_Comment by @lgig on 2024-09-18 06:58_

I didn't and that was it. I see it's also mentioned in the tutorial, but in a hurry I still missed it, my bad. Apologies for the silly question and thank you. :)

---

_Closed by @lgig on 2024-09-18 06:58_

---
