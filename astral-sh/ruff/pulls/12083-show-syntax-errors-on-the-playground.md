```yaml
number: 12083
title: Show syntax errors on the playground
type: pull_request
state: merged
author: dhruvmanila
labels:
  - playground
assignees: []
merged: true
base: main
head: dhruv/playground-syntax-errors
created_at: 2024-06-28T05:33:57Z
updated_at: 2024-06-28T07:36:17Z
url: https://github.com/astral-sh/ruff/pull/12083
synced_at: 2026-01-10T21:56:00Z
```

# Show syntax errors on the playground

---

_Pull request opened by @dhruvmanila on 2024-06-28 05:33_

## Summary

This PR updates the playground to show syntax errors.

(I forgot to update this and noticed it this morning.)

## Test Plan

Build the playground locally and preview it:

<img width="764" alt="Screenshot 2024-06-28 at 11 03 35" src="https://github.com/astral-sh/ruff/assets/67177269/1fd48d6c-ae41-4672-bf3c-32a61d9946ef">



---

_Label `playground` added by @dhruvmanila on 2024-06-28 05:33_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-28 05:39_

---

_Comment by @github-actions[bot] on 2024-06-28 05:47_

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

_@MichaReiser approved on 2024-06-28 07:02_

---

_Merged by @dhruvmanila on 2024-06-28 07:36_

---

_Closed by @dhruvmanila on 2024-06-28 07:36_

---

_Branch deleted on 2024-06-28 07:36_

---
