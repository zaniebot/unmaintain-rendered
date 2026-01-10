```yaml
number: 9643
title: "Delete `is_node_with_body` method"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: delete-is-node-with-body
created_at: 2024-01-25T14:32:47Z
updated_at: 2024-01-25T14:50:42Z
url: https://github.com/astral-sh/ruff/pull/9643
synced_at: 2026-01-10T22:57:09Z
```

# Delete `is_node_with_body` method

---

_Pull request opened by @MichaReiser on 2024-01-25 14:32_

## Summary

I noticed that the `is_node_with_body` function returns false for `WithItem` and `MatchCase` which looks like a bug. 

I searched for where the function is used to add tests showing that returning `true` for these two nodes has the desired result and noticed the function isn't used. 

This PR removes the unused function.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-01-25 14:33_

---

_Comment by @charliermarsh on 2024-01-25 14:33_

_Need_ dead code detection for workspaces

---

_Merged by @MichaReiser on 2024-01-25 14:41_

---

_Closed by @MichaReiser on 2024-01-25 14:41_

---

_Branch deleted on 2024-01-25 14:41_

---

_Comment by @github-actions[bot] on 2024-01-25 14:50_

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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
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
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---
