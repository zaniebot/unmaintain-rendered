```yaml
number: 19361
title: Add shellcheck to pre-commit
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/shellcheck-pre-commit
created_at: 2025-07-15T16:30:58Z
updated_at: 2025-07-15T16:55:51Z
url: https://github.com/astral-sh/ruff/pull/19361
synced_at: 2026-01-12T15:56:37Z
```

# Add shellcheck to pre-commit

---

_@AlexWaygood_

## Summary

We have a growing number of standalone shell scripts and https://github.com/astral-sh/ruff/pull/19360 proposes to add another one. We currently don't have any linters run in CI that check the syntax of these scripts, though. This PR proposes adding shellcheck to our pre-commit file to fix that hole.

## Test Plan

`uvx pre-commit run -a shellcheck`


---

_Label `ci` added by @AlexWaygood on 2025-07-15 16:30_

---

_@MichaReiser reviewed on 2025-07-15 16:35_

---

_Review comment by @MichaReiser on `fuzz/init-fuzzer.sh`:6 on 2025-07-15 16:35_

Should we add `set -eu` instead?

---

_@AlexWaygood reviewed on 2025-07-15 16:36_

---

_Review comment by @AlexWaygood on `fuzz/init-fuzzer.sh`:6 on 2025-07-15 16:36_

yeah, I guess that makes sense

---

_@MichaReiser approved on 2025-07-15 16:38_

---

_Merged by @AlexWaygood on 2025-07-15 16:49_

---

_Closed by @AlexWaygood on 2025-07-15 16:49_

---

_Branch deleted on 2025-07-15 16:49_

---

_Comment by @github-actions[bot] on 2025-07-15 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/mcp/databricks_mcp_cookbook.ipynb:12:1:8: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---
