```yaml
number: 3555
title: UP006 does not report type annotations in quotes
type: issue
state: closed
author: roikoren755
labels:
  - bug
assignees: []
created_at: 2023-03-16T08:16:48Z
updated_at: 2023-03-20T15:15:46Z
url: https://github.com/astral-sh/ruff/issues/3555
synced_at: 2026-01-12T15:54:43Z
```

# UP006 does not report type annotations in quotes

---

_@roikoren755_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
First of all, thank you so much for a great linter! We recently moved from `flake8` to `ruff`, which massively sped up our linting step, and allowed us to check for things we couldn't previously use `flake8` plugins for, like `flake8-type-checking`, due to having to support Python 3.6. This just worked with `ruff`, without any hassle! 👏 

We recently updated one of our repositories to a newer version of Python, and wanted to turn on UP006, specifically, when we ran into what we believe is a bug.
If the typing annotation that includes a generic from typing that is no longer necessary, according to PEP585, the UP006 rule does not flag it as an error.

```python
from typing import Dict

does_not_flag: "Dict[str, str]" = {}
does_flag: Dict[str, str] = {}
```

The command used to run is `ruff check up006.py --isolated --select UP006`, and the output of `ruff --version` is `ruff 0.0.256`.

Once again, thanks for all your work!

---

_Comment by @charliermarsh on 2023-03-16 15:24_

Thanks for the kind words :)

I _think_ we can check and fix these but I need to do some research -- there are some unintuitive corner cases around quoted annotations. We should almost certainly be flagging them as errors though, even if we can't autofix them.

(Related to #3508.)

---

_Label `bug` added by @charliermarsh on 2023-03-16 15:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-18 02:21_

---

_Comment by @charliermarsh on 2023-03-18 04:02_

For now, made sure that we flag these. In the future, we'll be able to autofix _some_ cases, but not all (e.g., this is valid, but hard to fix: `x: "Li" "st[int] = []`).


---

_Closed by @charliermarsh on 2023-03-20 15:15_

---
