```yaml
number: 5574
title: "`FA100` and `FA102` do not trigger with function annotations"
type: issue
state: closed
author: aazuspan
labels:
  - bug
assignees: []
created_at: 2023-07-07T00:12:50Z
updated_at: 2023-07-07T04:21:46Z
url: https://github.com/astral-sh/ruff/issues/5574
synced_at: 2026-01-10T11:09:48Z
```

# `FA100` and `FA102` do not trigger with function annotations

---

_Issue opened by @aazuspan on 2023-07-07 00:12_

I'm migrating a project to `ruff` (huge fan so far, thanks maintainers!), and getting inconsistent behavior from the `flake8-future-annotations` rules, [FA100](https://beta.ruff.rs/docs/rules/future-rewritable-type-annotation/#future-rewritable-type-annotation-fa100) and [FA102](https://beta.ruff.rs/docs/rules/future-required-type-annotation/#future-required-type-annotation-fa102). They trigger as expected for variable type annotations but do not trigger for function parameter or return type annotations.

## Reproducing

`pyproject.toml` config.
```toml
[tool.ruff]
target-version = "py38"
select = ["FA"]
```

`main.py` using the example code from the `ruff` docs.
```python
from typing import List


# Doesn't trigger FA100
def foo(a: List[str]):
    ...

# Doesn't trigger FA102
def bar(a: int | None):
    ...
```

And the commands I'm running:

```bash
$ ruff --version
0.0.277

$ ruff check ./main.py -v
[2023-07-06][17:10:49][ruff_cli::commands::run][DEBUG] Identified files to lint in: 2.948576ms
[2023-07-06][17:10:49][ruff_cli::diagnostics][DEBUG] Checking: /home/az/ruff_test/main.py
[2023-07-06][17:10:49][ruff_cli::commands::run][DEBUG] Checked 1 files in: 299.315µs
```

No rules triggered.

## Exceptions

Both rules *will* trigger with variable type annotations. For example:

`main.py`
```python
from typing import List


# Does trigger FA100
def foo():
    a: List[str]

# Does trigger FA102
def bar():
    a: int | None
```

```bash
$ ruff check ./main.py -v
[2023-07-06][17:10:02][ruff_cli::commands::run][DEBUG] Identified files to lint in: 1.427045ms
[2023-07-06][17:10:02][ruff_cli::diagnostics][DEBUG] Checking: /home/az/ruff_test/main.py
[2023-07-06][17:10:02][ruff_cli::commands::run][DEBUG] Checked 1 files in: 416.495µs
main.py:19:8: FA100 Missing `from __future__ import annotations`, but uses `typing.List`
main.py:23:8: FA102 Missing `from __future__ import annotations`, but uses PEP 604 union
Found 2 errors.
```

This seems to be inconsistent with `flake8-future-annotations`, the example code in the `ruff` docs, and I believe the intended behavior. Let me know if you need any more details, thanks!

---

_Comment by @charliermarsh on 2023-07-07 00:32_

Thanks for the clear issue. I need to look back to refresh myself on this.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-07 01:36_

---

_Label `bug` added by @charliermarsh on 2023-07-07 01:41_

---

_Comment by @charliermarsh on 2023-07-07 01:42_

Yeah this is a bug. Thanks! I'll make sure it's fixed for the next release.

---

_Comment by @aazuspan on 2023-07-07 01:56_

Awesome, thanks for such a quick response!

---

_Closed by @charliermarsh on 2023-07-07 04:21_

---
