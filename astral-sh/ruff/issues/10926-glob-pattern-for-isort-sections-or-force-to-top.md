```yaml
number: 10926
title: "Glob pattern for isort `sections` or `force-to-top`"
type: issue
state: open
author: serjflint
labels:
  - isort
  - needs-decision
assignees: []
created_at: 2024-04-14T14:55:47Z
updated_at: 2024-08-09T06:46:33Z
url: https://github.com/astral-sh/ruff/issues/10926
synced_at: 2026-01-10T11:09:53Z
```

# Glob pattern for isort `sections` or `force-to-top`

---

_Issue opened by @serjflint on 2024-04-14 14:55_

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
Hi!

ruff 0.3.4

I am trying to sort imports like `ruff check package/conftest.py --fix-only`

```python
import pytest

import package.pytest_init  # noqa: F401

from package import b
from package import a
```

I have a reason why "pytest_init" should be imported before other `first-party` imports. And I have many such packages.

If I sort as-is I get

```python
import pytest

from package import a
from package import b
import package.pytest_init  # noqa: F401
```

Which breaks some things for me.

The solution I found is to use `# isort: skip`


```python
import pytest

import package.pytest_init  # noqa: F401  # isort: skip

from package.sub import a
from package.sub import b
```

 or to add 

```toml
known-third-party = ["*.pytest_init"]
```

```python
import package.pytest_init  # noqa: F401
import pytest

from package import a
from package import b
```

Which keeps my code working, but puts `pytest_init` in an awkward place.

I wish to put "*.pytest_init" in its own section or at least at the top of `first-party`. Is it possible to add glob patterns to those parameters?

P.S.: I am not sure if `forced-separate` will work for my case.



---

_Label `isort` added by @AlexWaygood on 2024-04-14 16:10_

---

_Comment by @MichaReiser on 2024-04-15 06:58_

Have you tried [`section-order`](https://docs.astral.sh/ruff/settings/#lint_isort_section-order) with [`sections`](https://docs.astral.sh/ruff/settings/#lint_isort_sections)?

---

_Comment by @serjflint on 2024-04-15 09:17_

> Have you tried [`section-order`](https://docs.astral.sh/ruff/settings/#lint_isort_section-order) with [`sections`](https://docs.astral.sh/ruff/settings/#lint_isort_sections)?

Hi! Yes, I use it. 

The problem is that `package` is already mentioned in `first-party`. But I need `package.pytest_init` to be in a different section or at the top of `first-party`. 

I could write all such `package.pytest_init` manually, but with hundreds of them it would help to have a glob pattern.

---

_Comment by @charliermarsh on 2024-04-18 00:44_

These settings are only applied to the first segment of the import -- we don't classify beyond the top-level of the import anyway. So I don't think globs would map to the model here.

---

_Comment by @serjflint on 2024-04-18 06:36_

> These settings are only applied to the first segment of the import -- we don't classify beyond the top-level of the import anyway. So I don't think globs would map to the model here.

It works for `known-third-party`. Is it possible to spread that behavior?

It is a feature request. What should I do to be considered as such? It seems to me that you explain the current behavior.

---

_Comment by @serjflint on 2024-04-18 07:03_

Right now I use pathlib and tomlkit to emulate this feature by generating pyproject.toml programmatically. And pyproject.toml is several thousand lines long.

---

_Comment by @serjflint on 2024-08-08 17:58_

> Right now I use pathlib and tomlkit to emulate this feature by generating pyproject.toml programmatically. And pyproject.toml is several thousand lines long.

Hi! Listing all packages manually as a first-party, a third-party or a custom one helps with my issues. But it requires adding all of them with scripts. Is possible to add patterns like 'src/*' to isort sections?

---

_Label `needs-decision` added by @MichaReiser on 2024-08-09 06:46_

---
