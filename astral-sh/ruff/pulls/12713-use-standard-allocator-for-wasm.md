```yaml
number: 12713
title: Use standard allocator for wasm
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: remove-wee-alloc
created_at: 2024-08-06T11:16:06Z
updated_at: 2024-08-06T11:34:50Z
url: https://github.com/astral-sh/ruff/pull/12713
synced_at: 2026-01-12T15:55:42Z
```

# Use standard allocator for wasm

---

_@MichaReiser_

## Summary

This PR replaces `wee-alloc` with the std allocator. 

`wee-alloc` is advertised in many Rust WASM guides as a smaller allocator that's purpose built for WASM. 
However, `wee-alloc` is [unmaintained](https://github.com/rustsec/advisory-db/issues/1394) and [leaks memory](https://github.com/rustwasm/wee_alloc/issues/106)

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-08-06 11:16_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-06 11:16_

---

_Label `red-knot` added by @MichaReiser on 2024-08-06 11:16_

---

_Merged by @MichaReiser on 2024-08-06 11:20_

---

_Closed by @MichaReiser on 2024-08-06 11:20_

---

_Branch deleted on 2024-08-06 11:20_

---

_Comment by @github-actions[bot] on 2024-08-06 11:34_

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
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---
