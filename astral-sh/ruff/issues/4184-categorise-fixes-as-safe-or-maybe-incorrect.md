```yaml
number: 4184
title: "Categorise `fixes` as `safe` or `maybe_incorrect`"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
  - help wanted
assignees: []
created_at: 2023-05-02T09:18:33Z
updated_at: 2023-06-28T16:37:51Z
url: https://github.com/astral-sh/ruff/issues/4184
synced_at: 2026-01-10T11:09:47Z
```

# Categorise `fixes` as `safe` or `maybe_incorrect`

---

_Issue opened by @MichaReiser on 2023-05-02 09:18_

Part of #4181 and depends on #4183

#4183 introduced the new `Fix::safe` and `Fix::maybe_incorrect` constructors. The goal of this task is to go through all usages of  `Fix::unspecified`, `Fix::unspecified_edits`, `Diagnostic::try_set_from_edit`, and `Diagnostic::set_from_edit` and replace them with `Fix::safe*` or `Fix::maybe_incorrect` depending on whether the fix is safe or *unsafe* (may introduce semantic changes). 

I recommend creating individual PRs per rule because it makes discussing the safety properties of individual fixes easier. 

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:22_

---

_Label `internal` added by @MichaReiser on 2023-05-02 09:24_

---

_Comment by @zanieb on 2023-05-14 01:18_

Note the constructor names have changed:

- `Fix::safe` -> `Fix::automatic`
- `Fix::maybe_incorrect` -> `Fix::suggested` or `Fix::manual`
- `Diagnostic::try_set_from_edit` -> `Diagnostic::try_set_fix_from_edit`

See https://github.com/charliermarsh/ruff/pull/4303 and https://github.com/charliermarsh/ruff/pull/4275 for details.


---

_Comment by @sladyn98 on 2023-05-22 08:37_

@MichaReiser I was going through the codebase and it seems all of the code has been migrated, is there something that I am missing with this migration. 

---

_Comment by @MichaReiser on 2023-05-22 08:50_

> @MichaReiser I was going through the codebase and it seems all of the code has been migrated, is there something that I am missing with this migration.

You can search for usages of `Fix::unspecified` or `Fix::unspecified_edits` to find rules that need to be migrated

https://github.com/charliermarsh/ruff/blob/c6e5fed6587c95839a814ee8dfa845b465351229/crates/ruff_diagnostics/src/fix.rs#L40-L59

---

_Comment by @evanrittenhouse on 2023-06-15 19:59_

#5119  depends on all `Fix::unspecified` instances being refactored to an applicable (pun intended :P ) `Applicability`. Currently, `ripgrep` says there are occurrences in:
1. `flake8_commas`: fixed in #5127 
2. `flake8_logging_format`: fixed in #5129 
3. `flake8_pytest_style`: 
4. `flake8_quotes`: fixed in #5130 
5. `flake8_simpilfy`:
6. `flake8_tidy_imports`:
7. `flynt`
8. `isort`
9. `pandas_vet`
10. `pycodestyle`
11. `pydocstyle`
12. `pyflakes`
13. `pylint`
14. `pyupgrade`

An up-to-date list can be found in the description for #5119.

---

_Assigned to @evanrittenhouse by @MichaReiser on 2023-06-21 14:27_

---

_Closed by @charliermarsh on 2023-06-23 22:54_

---

_Comment by @evanrittenhouse on 2023-06-23 23:06_

@charliermarsh I believe there are two more linters that need to be completed - pycodestyle and flake8_pytest_style. May want to reopen until those are done, though I plan on completing them by the end of the weekend. 

---

_Reopened by @charliermarsh on 2023-06-23 23:20_

---

_Comment by @charliermarsh on 2023-06-23 23:20_

Oh sorry, did this get closed accidentally by a PR? Anyway, reopened!

---

_Comment by @zanieb on 2023-06-28 13:46_

I think this is done now! @evanrittenhouse let me know if that's wrong — thanks for taking all those on!

---

_Closed by @zanieb on 2023-06-28 13:46_

---

_Comment by @evanrittenhouse on 2023-06-28 13:57_

Yup! On to the fun part ;)

---

_Comment by @MichaReiser on 2023-06-28 16:37_

Wohoo. Nice work @evanrittenhouse. 

---
