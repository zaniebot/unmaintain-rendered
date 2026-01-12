```yaml
number: 19393
title: "[ty] Use `bitflags` for resolved client capabilities"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/bitflags-for-resolved-client-capabilities
created_at: 2025-07-17T08:33:47Z
updated_at: 2025-07-17T10:01:49Z
url: https://github.com/astral-sh/ruff/pull/19393
synced_at: 2026-01-12T15:56:38Z
```

# [ty] Use `bitflags` for resolved client capabilities

---

_@dhruvmanila_

## Summary

This PR updates the `ResolvedClientCapabilities` to be represented as `bitflags`. This allows us to remove the `Arc` as the type becomes copy. 

Additionally, this PR also fixed the goto definition and declaration code to use the `textDocument.definition.linkSupport` and `textDocument.declaration.linkSupport` client capability.

This PR also removes the unused client capabilities which are `code_action_deferred_edit_resolution`, `apply_edit`, and `document_changes` which are all related to auto-fix ability.


---

_Review requested from @carljm by @dhruvmanila on 2025-07-17 08:33_

---

_Label `internal` added by @dhruvmanila on 2025-07-17 08:33_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-17 08:33_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-17 08:33_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-17 08:33_

---

_Label `server` added by @dhruvmanila on 2025-07-17 08:33_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-17 08:33_

---

_Label `ty` added by @dhruvmanila on 2025-07-17 08:33_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-17 08:33_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-17 08:33_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-17 08:33_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-17 08:33_

---

_Comment by @github-actions[bot] on 2025-07-17 08:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-17 08:47_

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

_@MichaReiser approved on 2025-07-17 08:59_

---

_Merged by @dhruvmanila on 2025-07-17 10:01_

---

_Closed by @dhruvmanila on 2025-07-17 10:01_

---

_Branch deleted on 2025-07-17 10:01_

---
