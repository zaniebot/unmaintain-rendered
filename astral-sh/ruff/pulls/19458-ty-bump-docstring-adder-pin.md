```yaml
number: 19458
title: "[ty] bump docstring-adder pin"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/bump-docstring-adder
created_at: 2025-07-21T12:23:46Z
updated_at: 2025-07-21T12:38:42Z
url: https://github.com/astral-sh/ruff/pull/19458
synced_at: 2026-01-12T15:56:40Z
```

# [ty] bump docstring-adder pin

---

_@AlexWaygood_

## Summary

Bumps docstring-adder to https://github.com/astral-sh/docstring-adder/commit/6e91640a011248f5d9f85ce98218e16d1c4277c4, and uses some environment variables to reduce the number of hardcoded strings that are repeated many times across the workflow

## Test Plan

I'll kick off a typeshed-sync workflow run on this branch


---

_Label `internal` added by @AlexWaygood on 2025-07-21 12:23_

---

_Comment by @github-actions[bot] on 2025-07-21 12:35_

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

_@MichaReiser approved on 2025-07-21 12:37_

---

_Merged by @AlexWaygood on 2025-07-21 12:38_

---

_Closed by @AlexWaygood on 2025-07-21 12:38_

---

_Branch deleted on 2025-07-21 12:38_

---
