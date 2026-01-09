---
number: 6471
title: merging import lines like isort
type: issue
state: closed
author: nijel
labels:
  - needs-info
assignees: []
created_at: 2023-08-10T06:03:02Z
updated_at: 2023-08-10T18:28:04Z
url: https://github.com/astral-sh/ruff/issues/6471
synced_at: 2026-01-07T13:12:15-06:00
---

# merging import lines like isort

---

_Issue opened by @nijel on 2023-08-10 06:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

After some refactoring, several imports were removed and this was left:

```python
from foo import (
    bar,
    baz,
)

bar()
baz()
```

With isort this is nicely formatted as:

```python
from foo import bar, baz

bar()
baz()
```

But ruff does no formatting here and keeps the file as is.

Ruff 0.0.284 was executed as `ruff check --fix --select I test.py` with no config, isort as `isort test.py` with not config.



---

_Comment by @charliermarsh on 2023-08-10 12:51_

I think you’re looking for this setting: https://beta.ruff.rs/docs/settings/#isort-split-on-trailing-comma. Ruff defaults it to true while isort does not. (It’s a point of confusion as the isort docs state it defaults to true for their Black configuration, but doesn’t seem to do so in practice.)

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-10 12:51_

---

_Comment by @nijel on 2023-08-10 18:28_

Thanks, that's it. While reading through the doc, I didn't realize that something called `split` will actually toggle folding. 


And isort documentation is indeed wrong on this, I've created https://github.com/PyCQA/isort/pull/2163 for that.

---

_Closed by @nijel on 2023-08-10 18:28_

---

_Referenced in [python/typeshed#10912](../../python/typeshed/pulls/10912.md) on 2023-10-19 14:54_

---
