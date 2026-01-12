```yaml
number: 15197
title: "[docs] improve and fix entry for `analyze.include-dependencies`"
type: pull_request
state: merged
author: purajit
labels:
  - documentation
assignees: []
merged: true
base: main
head: 20241230-include-dependencies-docs
created_at: 2024-12-30T07:04:32Z
updated_at: 2024-12-31T19:45:32Z
url: https://github.com/astral-sh/ruff/pull/15197
synced_at: 2026-01-12T15:55:50Z
```

# [docs] improve and fix entry for `analyze.include-dependencies`

---

_@purajit_

## Summary

Changes two things about the entry:
* make the example valid TOML - inline tables must be a single line, at least till v1.1.0 is released, 
but also while in the future the toml version used by ruff might handle it, it would probably be 
good to stick to a spec that's readable by the vast majority of other tools and versions as well, 
especially if people are using `pyproject.toml`. The current example leads to `ruff` failure.
See https://github.com/toml-lang/toml/pull/904
* adds a line about the ability to add non-Python files to the map, which I think is a specific and 
important feature people should know about (in fact, I would assume this could potentially 
become the single biggest use-case for this).

## Test Plan

Ran doc creation as described in the [contribution](https://docs.astral.sh/ruff/contributing/#mkdocs) guide.

---

_Renamed from "[docs] improve and fix entry for analyze.include-dependencies" to "[docs] improve and fix entry for `analyze.include-dependencies`" by @purajit on 2024-12-30 07:04_

---

_Comment by @purajit on 2024-12-30 07:11_

new entry
<img width="987" alt="image" src="https://github.com/user-attachments/assets/98d10024-1efd-43b0-ba29-e8e1849e5e5e" />


---

_Comment by @github-actions[bot] on 2024-12-30 07:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-12-30 08:06_

---

_Label `documentation` added by @MichaReiser on 2024-12-30 08:06_

---

_Comment by @purajit on 2024-12-30 09:09_

For the second point, I was trying to see if that was intentional or not; couldn't find anything
in the original PR, and the code
https://github.com/astral-sh/ruff/blob/8a98d888474a1c1a5bff61fe94f8d875c1a4ccb2/crates/ruff/src/commands/analyze_graph.rs#L156-L159
doesn't suggest anything strongly either; `GlobResolver` resolves all files. My main hesitation 
is that it's consistently called an "import map", and there are already issues/discussions about
expanding the scope of `analyze graph` assuming that these are only Python deps.. I hope it 
stays, though I understand if it won't.

---

_@charliermarsh reviewed on 2024-12-30 15:40_

Thanks! It's an intentional behavior.

---

_Merged by @charliermarsh on 2024-12-30 15:45_

---

_Closed by @charliermarsh on 2024-12-30 15:45_

---

_Branch deleted on 2024-12-31 19:45_

---
