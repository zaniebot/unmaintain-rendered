---
number: 10266
title: isort counts third party imports as local when inside submodules with the same name
type: issue
state: open
author: mmerickel
labels:
  - isort
assignees: []
created_at: 2024-03-07T06:16:50Z
updated_at: 2024-03-11T17:01:25Z
url: https://github.com/astral-sh/ruff/issues/10266
synced_at: 2026-01-07T13:12:15-06:00
---

# isort counts third party imports as local when inside submodules with the same name

---

_Issue opened by @mmerickel on 2024-03-07 06:16_

## repro example
```
.
├── pyproject.toml
└── src
    └── ruff_test
        └── setuptools
            ├── __init__.py
            └── example.py
```

### pyproject.toml
```toml
[project]
name = "ruff-test"
version = "0.1.0"
description = "Add your description here"
authors = [
    { name = "Michael Merickel", email = "unknown@domain.invalid" }
]
dependencies = []
readme = "README.md"
requires-python = ">= 3.8"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.rye]
managed = true
dev-dependencies = []

[tool.hatch.metadata]
allow-direct-references = true

[tool.hatch.build.targets.wheel]
packages = ["src/ruff_test"]

[tool.ruff]
src = ["src"]

[tool.ruff.lint]
select = ["I"]
```

### example.py
```python
import setuptools
```

### ruff check output
```
❯ ruff check -v --no-cache src/ruff_test/setuptools/example.py
[2024-03-06][23:01:54][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/michael.merickel/scratch/ruff-test/pyproject.toml
[2024-03-06][23:01:54][ruff::commands::check][DEBUG] Identified files to lint in: 2.003584ms
[2024-03-06][23:01:54][ruff::diagnostics][DEBUG] Checking: /Users/michael.merickel/scratch/ruff-test/src/ruff_test/setuptools/example.py
[2024-03-06][23:01:54][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'setuptools' as Known(FirstParty) (SamePackage)
[2024-03-06][23:01:54][ruff::commands::check][DEBUG] Checked 1 files in: 500.583µs
```

## explanation of problem
> Categorized 'setuptools' as Known(FirstParty) (SamePackage)

`import setuptools` here is an absolute import and should be counted as `Known(ThirdParty)`.

The isort logic appears to be counting this as a first party import in a very specific circumstance. The critical detail here is that `ruff_test` is a namespace package and has no `__init__.py` file. If you `touch src/ruff_test/__init__.py` then you get the expected output `Categorized 'setuptools' as Known(ThirdParty) (NoMatch)`.

Instead of adding the `__init__.py` it is also possible to define a `known-third-party` section to solve this issue:

```toml
[tool.ruff.lint.isort]
known-third-party = ["setuptools"]
```

Then you will see `Categorized 'setuptools' as Known(ThirdParty) (KnownThirdParty)`.

If you remove the `src` declaration from `tool.ruff` it is still a problem, but I think the `src` declaration should make it extra clear to ruff that the absolute import of the package we created is `ruff_test.setuptools`, and not `setuptools`.

If ruff knew the appropriate top-level package roots then `import setuptools` would be correctly matched to a third party package because "absolute relative imports" are no longer a thing in Python 3.

Another workaround is setting `detect-same-package = false`. Another option is also `namespace-packages = ["src/ruff_test"]`.

I opened this issue because I think the fact that I set `src = ["src"]` should have been enough info for ruff to do the right thing in the original `pyproject.toml`.

I tested this on ruff 0.3.0 and 0.3.1 and observed the same behavior on both.

Possibly related: https://github.com/astral-sh/ruff/issues/5667 https://github.com/astral-sh/ruff/issues/9130

---

_Label `isort` added by @MichaReiser on 2024-03-07 08:14_

---

_Comment by @zanieb on 2024-03-11 17:01_

Thanks for the report!

@charliermarsh do we just not support detection of first-party imports in namespace packages in general?

---

_Referenced in [astral-sh/ruff#10989](../../astral-sh/ruff/issues/10989.md) on 2024-04-17 07:12_

---

_Referenced in [pypa/pip#12937](../../pypa/pip/pulls/12937.md) on 2024-08-24 20:23_

---

_Referenced in [monarch-initiative/curategpt#72](../../monarch-initiative/curategpt/pulls/72.md) on 2024-08-28 22:12_

---

_Referenced in [astral-sh/ruff#14497](../../astral-sh/ruff/issues/14497.md) on 2024-11-20 20:08_

---
