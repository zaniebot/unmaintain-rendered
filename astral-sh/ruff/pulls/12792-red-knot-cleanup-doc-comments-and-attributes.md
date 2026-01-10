```yaml
number: 12792
title: "[red-knot] cleanup doc comments and attributes"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cleanup
created_at: 2024-08-09T23:28:46Z
updated_at: 2024-08-12T19:15:18Z
url: https://github.com/astral-sh/ruff/pull/12792
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] cleanup doc comments and attributes

---

_Pull request opened by @carljm on 2024-08-09 23:28_

Make `cargo doc -p red_knot_python_semantic --document-private-items` run warning-free. I'd still like to do this for all of ruff and start enforcing it in CI (https://github.com/astral-sh/ruff/issues/12372) but haven't gotten to it yet. But in the meantime I'm trying to maintain it for at least `red_knot_python_semantic`, as it helps to ensure our doc comments stay up to date.

A few of the comments I just removed or shortened, as their continued relevance wasn't clear to me; please object in review if you think some of them are important to keep!

Also remove a no-longer-needed `allow` attribute.

(This is just a collection of cleanups I made in passing while working on type narrowing; splitting them out to reduce the size of that PR.)


---

_Label `red-knot` added by @carljm on 2024-08-09 23:28_

---

_Review requested from @MichaReiser by @carljm on 2024-08-09 23:28_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-09 23:28_

---

_@AlexWaygood approved on 2024-08-09 23:35_

---

_Comment by @github-actions[bot] on 2024-08-09 23:42_

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

_@MichaReiser approved on 2024-08-12 07:44_

---

_Comment by @MichaReiser on 2024-08-12 07:44_

You could consider adding `cargo doc -p red_knot_python_semantic --document-private-items` to the CI step to prevent new regressions. 



---

_Merged by @carljm on 2024-08-12 19:15_

---

_Closed by @carljm on 2024-08-12 19:15_

---

_Branch deleted on 2024-08-12 19:15_

---
