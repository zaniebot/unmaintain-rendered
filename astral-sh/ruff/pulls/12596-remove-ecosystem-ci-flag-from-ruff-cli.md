```yaml
number: 12596
title: "Remove `ecosystem_ci` flag from Ruff CLI"
type: pull_request
state: merged
author: chriskrycho
labels:
  - internal
assignees: []
merged: true
base: main
head: drop_ecosystem_ci
created_at: 2024-07-31T15:44:32Z
updated_at: 2024-07-31T16:58:15Z
url: https://github.com/astral-sh/ruff/pull/12596
synced_at: 2026-01-10T21:47:02Z
```

# Remove `ecosystem_ci` flag from Ruff CLI

---

_Pull request opened by @chriskrycho on 2024-07-31 15:44_

## Summary

@zanieb noticed while we were discussing #12595 that this flag is now unnecessary, so remove it and the flags which reference it.

## Test Plan

Question for maintainers: is there a test to add *or* remove here? (I’ve opened this as a draft PR with that in view!)


---

_Comment by @github-actions[bot] on 2024-07-31 16:02_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_Notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_Notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_@charliermarsh approved on 2024-07-31 16:12_

Thanks! Should be fine as long as the ecosystem checks pass.

---

_Label `internal` added by @zanieb on 2024-07-31 16:39_

---

_Marked ready for review by @zanieb on 2024-07-31 16:39_

---

_Merged by @zanieb on 2024-07-31 16:40_

---

_Closed by @zanieb on 2024-07-31 16:40_

---

_Branch deleted on 2024-07-31 16:58_

---
