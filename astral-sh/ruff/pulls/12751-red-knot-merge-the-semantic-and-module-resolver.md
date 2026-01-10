```yaml
number: 12751
title: "[red-knot] Merge the semantic and module-resolver crates"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/merge-crates
created_at: 2024-08-08T12:36:27Z
updated_at: 2024-08-08T14:34:13Z
url: https://github.com/astral-sh/ruff/pull/12751
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] Merge the semantic and module-resolver crates

---

_Pull request opened by @AlexWaygood on 2024-08-08 12:36_

## Summary

This PR merges the `red_knot_python_semantic` and `red_knot_module_resolver` crates. Since this involves moving quite a few files, this PR _only_ merges the two crates, and doesn't do anything else. The module resolver becomes a self-contained submodule of `red_knot_python_semantic`, except for `module_name.rs`, which becomes a top-level submodule of `red_knot_python_semantic`.

(None of the vendored typeshed files have changed in any way, only moved. You should be able to scroll past them when looking at the diff.)

The motivation for this is that it should make it easier to validate settings for search paths eagerly, outside of Salsa.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-08 12:36_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-08 12:36_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-08 12:36_

---

_Comment by @github-actions[bot] on 2024-08-08 12:58_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
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

_@MichaReiser approved on 2024-08-08 14:05_

Nice!

---

_Merged by @AlexWaygood on 2024-08-08 14:34_

---

_Closed by @AlexWaygood on 2024-08-08 14:34_

---

_Branch deleted on 2024-08-08 14:34_

---
