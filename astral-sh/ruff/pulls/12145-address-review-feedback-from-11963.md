```yaml
number: 12145
title: Address review feedback from 11963
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: semantic-model-api-cleanup
created_at: 2024-07-02T06:54:24Z
updated_at: 2024-07-02T07:07:36Z
url: https://github.com/astral-sh/ruff/pull/12145
synced_at: 2026-01-12T15:55:40Z
```

# Address review feedback from 11963

---

_@MichaReiser_

## Summary

I messed up the last push on https://github.com/astral-sh/ruff/pull/11963 which reverted all the changes that addressed the code review feedback (I addressed those from my notebook but I rebased pushed from my desktop).

This PR re-adds the changes. 

I'll merge this PR without review because they changes have already been reviewed as part of https://github.com/astral-sh/ruff/pull/11963
	
## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-07-02 06:54_

---

_Merged by @MichaReiser on 2024-07-02 07:05_

---

_Closed by @MichaReiser on 2024-07-02 07:05_

---

_Branch deleted on 2024-07-02 07:05_

---

_Comment by @github-actions[bot] on 2024-07-02 07:07_

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
