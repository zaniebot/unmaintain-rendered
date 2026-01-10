```yaml
number: 12116
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2024-07-01T00:26:13Z
updated_at: 2024-07-01T06:07:46Z
url: https://github.com/astral-sh/ruff/pull/12116
synced_at: 2026-01-10T21:56:00Z
```

# Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2024-07-01 00:26_

Close and reopen this PR to trigger CI

---

_Label `internal` added by @github-actions[bot] on 2024-07-01 00:26_

---

_Closed by @charliermarsh on 2024-07-01 00:55_

---

_Reopened by @charliermarsh on 2024-07-01 00:55_

---

_Comment by @github-actions[bot] on 2024-07-01 01:09_

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

_Review requested from @AlexWaygood by @charliermarsh on 2024-07-01 01:57_

---

_@AlexWaygood approved on 2024-07-01 06:07_

---

_Merged by @AlexWaygood on 2024-07-01 06:07_

---

_Closed by @AlexWaygood on 2024-07-01 06:07_

---

_Branch deleted on 2024-07-01 06:07_

---
