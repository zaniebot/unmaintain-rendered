```yaml
number: 12629
title: Release Ruff 0.5.6
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: ruff-0.5.6
created_at: 2024-08-02T13:48:57Z
updated_at: 2024-08-02T15:37:29Z
url: https://github.com/astral-sh/ruff/pull/12629
synced_at: 2026-01-10T21:47:02Z
```

# Release Ruff 0.5.6

---

_Pull request opened by @MichaReiser on 2024-08-02 13:48_

_No description provided._

---

_Label `release` added by @MichaReiser on 2024-08-02 13:49_

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2024-08-02 14:00_

```suggestion
- \[`flake8-implicit-str-concat`\] Always allow explicit multi-line concatenations when implicit concatenations are banned ([#12532](https://github.com/astral-sh/ruff/pull/12532))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:30 on 2024-08-02 14:03_

```suggestion
- \[`flake8-async`\] Avoid flagging `asyncio.timeout`s as unused when the context manager includes `asyncio.TaskGroup` ([#12605](https://github.com/astral-sh/ruff/pull/12605))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:46 on 2024-08-02 14:04_

```suggestion
```

---

_@AlexWaygood approved on 2024-08-02 14:04_

---

_Comment by @github-actions[bot] on 2024-08-02 14:13_

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

_@dhruvmanila reviewed on 2024-08-02 15:12_

---

_Review comment by @dhruvmanila on `CHANGELOG.md`:6 on 2024-08-02 15:12_

We could provide an example for this?
```toml
[tool.ruff]
extend-exclude = ["*.ipynb"]
```

---

_Comment by @MichaReiser on 2024-08-02 15:17_

I'm sure pre-commit will yell at me again for changing the markdown

---

_Merged by @MichaReiser on 2024-08-02 15:35_

---

_Closed by @MichaReiser on 2024-08-02 15:35_

---

_Branch deleted on 2024-08-02 15:35_

---
