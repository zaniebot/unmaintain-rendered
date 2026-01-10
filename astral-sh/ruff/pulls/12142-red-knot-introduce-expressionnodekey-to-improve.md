```yaml
number: 12142
title: "[red-knot] Introduce `ExpressionNodeKey` to improve typing of `expression_map`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: ast-ids-expression-ref
created_at: 2024-07-01T13:53:40Z
updated_at: 2024-07-01T14:15:55Z
url: https://github.com/astral-sh/ruff/pull/12142
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] Introduce `ExpressionNodeKey` to improve typing of `expression_map`

---

_Pull request opened by @MichaReiser on 2024-07-01 13:53_

## Summary

Small refactor that used to be part of https://github.com/astral-sh/ruff/pull/11971

This PR introduces an `ExpressionNodeKey` that can only be constructed from an `Expr` and uses that as key into the `expression_map`. 
Using `ExpressionNodeKey` encodes the restriction that the map should only ever contain expression keys and using anything else won't compile.

We could do the same for `statements_map` but this is going to change more significantely, that's why I'm leaving it as is for now.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-07-01 13:53_

---

_Comment by @github-actions[bot] on 2024-07-01 14:09_

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

_Merged by @MichaReiser on 2024-07-01 14:15_

---

_Closed by @MichaReiser on 2024-07-01 14:15_

---

_Branch deleted on 2024-07-01 14:15_

---
