```yaml
number: 12139
title: "Refactor `ast_ids` traits to take `ScopeId` instead of `VfsFile` plus `FileScopeId`."
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: ast-ids-scope-argument
created_at: 2024-07-01T13:39:22Z
updated_at: 2024-07-01T13:52:31Z
url: https://github.com/astral-sh/ruff/pull/12139
synced_at: 2026-01-10T21:56:00Z
```

# Refactor `ast_ids` traits to take `ScopeId` instead of `VfsFile` plus `FileScopeId`.

---

_Pull request opened by @MichaReiser on 2024-07-01 13:39_

## Summary

This is a small refactor that changes the `ast_ids` traits to take a `ScopeId` argument instead of a `VfsFile` and `FileScopeId`. 

This is mainly to reduce the number of arguments, but it might even improve performance because most call-sites already have the `ScopeId` and previously it was necessary to lookup the `ScopeId`.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-07-01 13:39_

---

_Comment by @MichaReiser on 2024-07-01 13:47_

This is a smaller refactor that I extracted from https://github.com/astral-sh/ruff/pull/11971 I'll go ahead with the merge as soon as all checks pass. 

---

_Merged by @MichaReiser on 2024-07-01 13:50_

---

_Closed by @MichaReiser on 2024-07-01 13:50_

---

_Branch deleted on 2024-07-01 13:50_

---

_Comment by @github-actions[bot] on 2024-07-01 13:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---
