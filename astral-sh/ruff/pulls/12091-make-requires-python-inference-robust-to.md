```yaml
number: 12091
title: "Make `requires-python` inference robust to `==`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/detect
created_at: 2024-06-28T12:08:00Z
updated_at: 2024-06-28T13:38:18Z
url: https://github.com/astral-sh/ruff/pull/12091
synced_at: 2026-01-12T15:55:40Z
```

# Make `requires-python` inference robust to `==`

---

_@charliermarsh_

## Summary

Instead of using a high patch version, attempt to detect the minimum-supported minor.

Closes #12088.


---

_Review requested from @konstin by @charliermarsh on 2024-06-28 12:08_

---

_Label `bug` added by @charliermarsh on 2024-06-28 12:08_

---

_Comment by @github-actions[bot] on 2024-06-28 12:21_

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

_@konstin approved on 2024-06-28 13:30_

---

_Merged by @charliermarsh on 2024-06-28 13:38_

---

_Closed by @charliermarsh on 2024-06-28 13:38_

---

_Branch deleted on 2024-06-28 13:38_

---
