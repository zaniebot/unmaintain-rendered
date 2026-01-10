```yaml
number: 12137
title: "[red-knot] Remove `Scope::name`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: remove-scope-name
created_at: 2024-07-01T12:50:44Z
updated_at: 2024-07-01T13:55:52Z
url: https://github.com/astral-sh/ruff/pull/12137
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Remove `Scope::name`

---

_Pull request opened by @MichaReiser on 2024-07-01 12:50_

## Summary

This PR removes `Scope::name` because it is currently unused outside tests and it requires cloning the names of every symbol and class which can get expensive. 

I can see how having a `name` might be useful for debugging. I'm open to have a *debug-only* `name` stored on `Scope` but we should then design it that way 
and also avoid writing tests around it. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-07-01 12:50_

---

_Review requested from @carljm by @MichaReiser on 2024-07-01 12:50_

---

_Comment by @github-actions[bot] on 2024-07-01 13:03_

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

_Merged by @MichaReiser on 2024-07-01 13:55_

---

_Closed by @MichaReiser on 2024-07-01 13:55_

---

_Branch deleted on 2024-07-01 13:55_

---
