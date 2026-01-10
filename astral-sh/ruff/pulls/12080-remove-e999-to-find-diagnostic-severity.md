```yaml
number: 12080
title: "Remove `E999` to find diagnostic severity"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/severity
created_at: 2024-06-28T03:36:56Z
updated_at: 2024-06-28T04:01:36Z
url: https://github.com/astral-sh/ruff/pull/12080
synced_at: 2026-01-10T21:56:00Z
```

# Remove `E999` to find diagnostic severity

---

_Pull request opened by @dhruvmanila on 2024-06-28 03:36_

## Summary

This PR removes the need to check for `E999` code to find the diagnostic severity in the server.

**Note:** This is just removing a redundant check because all `ParseErrors` are converted to `Diagnostic` with default `Error` severity by https://github.com/astral-sh/ruff/blob/63c92586a10bfa9b75db9cb87a9ac08618a2ed95/crates/ruff_server/src/lint.rs#L309-L346

## Test Plan

Verify that syntax errors are still shown with error severity as it did before:

<img width="1313" alt="Screenshot 2024-06-28 at 09 30 20" src="https://github.com/astral-sh/ruff/assets/67177269/75e389a7-01ea-461c-86a2-0dfc244e515d">


---

_Label `internal` added by @dhruvmanila on 2024-06-28 03:36_

---

_Comment by @github-actions[bot] on 2024-06-28 03:50_

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

_Merged by @dhruvmanila on 2024-06-28 04:01_

---

_Closed by @dhruvmanila on 2024-06-28 04:01_

---

_Branch deleted on 2024-06-28 04:01_

---
