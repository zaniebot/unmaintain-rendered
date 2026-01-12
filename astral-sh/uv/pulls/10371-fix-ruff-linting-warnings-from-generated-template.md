```yaml
number: 10371
title: Fix ruff linting warnings from generated template files for extension modules
type: pull_request
state: merged
author: kmarchais
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/ruff-compliance-init-templates
created_at: 2025-01-07T16:55:16Z
updated_at: 2025-01-07T17:07:44Z
url: https://github.com/astral-sh/uv/pull/10371
synced_at: 2026-01-12T16:09:15Z
```

# Fix ruff linting warnings from generated template files for extension modules

---

_@kmarchais_

## Summary

This PR fixes two ruff linting issues in the generated template files when using: `uv init --build-backend` for extension modules.

1. Removes unnecessary `from __future__ import annotations` imports from generated .pyi files ([PYI044](https://docs.astral.sh/ruff/rules/future-annotations-in-stub/))
2. Adds missing blank line after `hello_from_bin` import to comply with isort formatting ([I001](https://docs.astral.sh/ruff/rules/unsorted-imports/))

## Test Plan

```bash
cargo run -- init --build-backend scikit-build-core example-ext
uvx ruff check example-ext --select ALL

cargo run -- init --build-backend maturin example-ext
uvx ruff check example-ext --select ALL
```

## Remaining warnings

There are still warnings remainings in the generated `__init__.py` files:
- [D104](https://docs.astral.sh/ruff/rules/undocumented-public-package/) Missing docstring in public package
- [D103](https://docs.astral.sh/ruff/rules/undocumented-public-function/) Missing docstring in public function
- [T201](https://docs.astral.sh/ruff/rules/print/) `print` found

---

_@charliermarsh approved on 2025-01-07 16:55_

Thanks!

---

_Label `bug` added by @charliermarsh on 2025-01-07 16:57_

---

_Merged by @charliermarsh on 2025-01-07 17:07_

---

_Closed by @charliermarsh on 2025-01-07 17:07_

---
