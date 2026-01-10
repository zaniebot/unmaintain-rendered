```yaml
number: 10541
title: Nested namespace packages support
type: pull_request
state: merged
author: dizzy57
labels:
  - isort
assignees: []
merged: true
base: main
head: nested-namespace-packages
created_at: 2024-03-24T08:34:47Z
updated_at: 2024-03-27T18:41:16Z
url: https://github.com/astral-sh/ruff/pull/10541
synced_at: 2026-01-10T22:47:02Z
```

# Nested namespace packages support

---

_Pull request opened by @dizzy57 on 2024-03-24 08:34_

## Summary
PEP 420 says [nested namespace packages](https://peps.python.org/pep-0420/#nested-namespace-packages) are allowed, i.e. marking a directory as a namespace package marks all subdirectories in the subtree as namespace packages.

`is_package` is modified to use `Path::starts_with` and the order of checks is reversed to do in-memory checks first before hitting the disk.

## Test Plan
Added unit tests. Previously all tests were run with `namespace_packages == &[]`. Verified that one of the tests was failing before changing the implementation.

## Future Improvements
The `is_package_with_cache` can probably be rewritten to avoid repeated calls to `Path::starts_with`, by caching all directories up to the `namespace_root`:
```ruff
let namespace_root = namespace_packages
    .iter()
    .filter(|namespace_package| path.starts_with(namespace_package))
    .min();
```

---

_Comment by @dizzy57 on 2024-03-24 08:43_

Related: #6114 #5149

---

_Comment by @github-actions[bot] on 2024-03-24 08:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-24 20:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-24 20:07_

---

_Comment by @charliermarsh on 2024-03-24 20:11_

Did you run into an issue with this in practice that motivated this change?

---

_Comment by @dizzy57 on 2024-03-24 20:47_

> Did you run into an issue with this in practice that motivated this change?

We have a monorepo where all the files are located in an `organization/project/subproject/` structure and both `organization/` and `organization/project/` directories are meant to be namespace packages, so no `__init__.py` in them.

We wanted to use [banned-api (TID251)](https://docs.astral.sh/ruff/rules/banned-api/), but for python sources it detected `subproject` as python package name. We didn't want to mark all the `organization/project/` directories as namespace packages (it would be brittle, as more projects might be added without changing the ruff config), but marking just `organization/` as a namespace package didn't work.

---

_Comment by @dizzy57 on 2024-03-24 21:12_

This test illustrates our situation pretty well. Currently ruff assigns package name `core` to this directory, but we want it to be recognized as `python_modules.core.core` with only `python_modules` being marked as namespace package.

https://github.com/dizzy57/ruff/blob/7607b2d64083c797aa334b3f2e767a2a12bf191b/crates/ruff_linter/src/packaging.rs#L111-L117

---

_@charliermarsh approved on 2024-03-25 02:52_

---

_Comment by @charliermarsh on 2024-03-25 02:53_

Okay thanks. I think this is correct.

---

_Label `isort` added by @charliermarsh on 2024-03-25 02:53_

---

_Merged by @charliermarsh on 2024-03-25 02:53_

---

_Closed by @charliermarsh on 2024-03-25 02:53_

---

_Branch deleted on 2024-03-27 18:41_

---
