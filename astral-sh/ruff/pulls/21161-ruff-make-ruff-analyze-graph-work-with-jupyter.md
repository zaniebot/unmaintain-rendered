```yaml
number: 21161
title: "[ruff]: Make `ruff analyze graph` work with jupyter notebooks"
type: pull_request
state: merged
author: gauthsvenkat
labels:
  - notebook
  - analyze
assignees: []
merged: true
base: main
head: feat/ruff-analyze-graph-jupyter-notebooks
created_at: 2025-10-31T13:04:47Z
updated_at: 2025-11-29T08:37:30Z
url: https://github.com/astral-sh/ruff/pull/21161
synced_at: 2026-01-12T15:57:17Z
```

# [ruff]: Make `ruff analyze graph` work with jupyter notebooks

---

_@gauthsvenkat_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
 `ruff analyze graph` skips jupyter notebooks. This MR addresses that issue so that jupyter notebooks are also considered in the output. 

Closes: #21099 

### Detailed Summary
`ruff analyze graph` has the following lines to skip if the source is a jupyter notebook.

https://github.com/astral-sh/ruff/blob/d9cab4d242a18102617d2a9eea1fc10a70c67a50/crates/ruff/src/commands/analyze_graph.rs#L130-L133

Removing these lines alone is not enough (see [this comment](https://github.com/astral-sh/ruff/issues/21099#issuecomment-3453135503) on the issue). The reason is because in `detect()` the file under consideration is read and parsed to be a `.py` file. 

https://github.com/astral-sh/ruff/blob/d9cab4d242a18102617d2a9eea1fc10a70c67a50/crates/ruff_graph/src/lib.rs#L25-L33

For notebooks, you'd expect this parsing to fail as a `.ipynb` and `.py` are different. However that is not the case as under the hood, contents of a jupyter notebook are JSON. What instead is happening is that the jupyter notebooks get parsed as  
a giant nested python dictionary.

### Fix

- `detect()` now takes two more arguments; `source`: the raw content and `source_type` to correctly parse both `.py` and `.ipynb` content.
- in `analyze_graph.rs` we extract the raw content correctly for both `.py` and `.ipynb`.

## Test Plan

All existing unit tests pass and added two new tests to make sure the jupyter notebook flow also works.

---

_Label `notebook` added by @amyreese on 2025-10-31 18:44_

---

_Label `analyze` added by @amyreese on 2025-10-31 18:44_

---

_Review comment by @amyreese on `crates/ruff/src/commands/analyze_graph.rs`:134 on 2025-10-31 18:46_

Does this part still need to be removed?

---

_@amyreese reviewed on 2025-10-31 18:49_

---

_@gauthsvenkat reviewed on 2025-10-31 19:03_

---

_Review comment by @gauthsvenkat on `crates/ruff/src/commands/analyze_graph.rs`:134 on 2025-10-31 19:03_

Damn, I'm dumb. Yeah I think it should be removed.

What's wild is it seems to work even with that code present. I'll take a look soon.


EDIT: Spoke too soon! I think I messed up the cherry picking or something. It is removed now

---

_Comment by @github-actions[bot] on 2025-10-31 19:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-10-31 21:23_

This is great. Thank you

---

_Merged by @MichaReiser on 2025-10-31 21:47_

---

_Closed by @MichaReiser on 2025-10-31 21:47_

---

_Branch deleted on 2025-11-29 08:37_

---
