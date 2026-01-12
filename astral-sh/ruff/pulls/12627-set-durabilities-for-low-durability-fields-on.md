```yaml
number: 12627
title: Set durabilities for low-durability fields on high-durability inputs
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: set-durability-for-low-durability-fields
created_at: 2024-08-02T12:36:14Z
updated_at: 2024-08-02T17:50:14Z
url: https://github.com/astral-sh/ruff/pull/12627
synced_at: 2026-01-12T15:55:41Z
```

# Set durabilities for low-durability fields on high-durability inputs

---

_@MichaReiser_

## Summary

This PR sets the durability of low-durability fields on structs with a high durability. 
This prevents that changing those files requires a deep verification for all revisions. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2024-08-02 12:37_

---

_Comment by @github-actions[bot] on 2024-08-02 12:55_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2024-08-02 13:19_

---

_Review requested from @carljm by @MichaReiser on 2024-08-02 13:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-02 13:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace.rs`:112 on 2024-08-02 14:11_

nit: I know it's not new to this PR, but I'd consider renaming the attribute on `Workspace` to be `open_fileset` rather than `open_file_set`. Looking throught this PR, I assumed `open_file_set_durability` set the durability for a single open file, rather than setting the durability for the set of currently open files. "Fileset" is a word, and I think it would be just as grammatical to refer to an "open fileset" as it would be to refer to an "open file set".

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files.rs`:277 on 2024-08-02 14:12_

For my education: what does the `#[default]` attribute do here?

---

_@AlexWaygood approved on 2024-08-02 14:12_

---

_@MichaReiser reviewed on 2024-08-02 15:11_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:277 on 2024-08-02 15:11_

Will see if it gets accepted. It initializes the value with `Default::default` unless it's specified explicilty

---

_@carljm approved on 2024-08-02 16:32_

---

_Merged by @MichaReiser on 2024-08-02 17:42_

---

_Closed by @MichaReiser on 2024-08-02 17:42_

---

_Branch deleted on 2024-08-02 17:42_

---
