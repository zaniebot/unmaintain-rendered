```yaml
number: 3370
title: "Create a `rust_python_ast` crate"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ruff_python_ast
created_at: 2023-03-06T22:18:05Z
updated_at: 2023-03-07T15:18:42Z
url: https://github.com/astral-sh/ruff/pull/3370
synced_at: 2026-01-12T15:55:12Z
```

# Create a `rust_python_ast` crate

---

_@charliermarsh_

This PR productionizes @MichaReiser's suggestion in https://github.com/charliermarsh/ruff/issues/1820#issuecomment-1440204423, by creating a separate crate for the `ast` module (`rust_python_ast`). This will enable us to further split up the `ruff` crate, as we'll be able to create (e.g.) separate sub-linter crates that have access to these common AST utilities.

This was mostly a straightforward copy (with adjustments to module imports), as the few dependencies that _did_ require modifications were handled in #3366, #3367, and #3368.


---

_Comment by @charliermarsh on 2023-03-06 22:42_

We could make this something more general, like `ruff_python_syntax`, as suggested in the linked issue, which would let us move more code into this crate (e.g., `docstrings`).

I actually went through with this change misremembering that suggestion. I don't feel strongly either way.

(If we use `ruff_python_syntax`, we probably want to then scope all this code under `ruff_python_syntax::ast`.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-06 23:08_

---

_Review requested from @konstin by @charliermarsh on 2023-03-06 23:08_

---

_@konstin approved on 2023-03-07 11:00_

---

_Merged by @charliermarsh on 2023-03-07 15:18_

---

_Closed by @charliermarsh on 2023-03-07 15:18_

---

_Branch deleted on 2023-03-07 15:18_

---
