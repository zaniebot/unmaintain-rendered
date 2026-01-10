```yaml
number: 12696
title: Add a new script to generate builtin module names
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: generate-builtin-modules
created_at: 2024-08-05T20:20:42Z
updated_at: 2024-08-05T20:38:54Z
url: https://github.com/astral-sh/ruff/pull/12696
synced_at: 2026-01-10T21:47:02Z
```

# Add a new script to generate builtin module names

---

_Pull request opened by @AlexWaygood on 2024-08-05 20:20_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12398.

This PR adds a Python script to generate a Rust function that can be used to determine whether a Python module is built into the Python interpreter on a given Python version. The script is heavily inspired by the already-existing [`generate_known_standard_library.py` script](https://github.com/astral-sh/ruff/blob/main/scripts/generate_known_standard_library.py).

The script doesn't have any third-party dependencies, but _does_ require that you have quite a few Pythons installed that can all be called using subprocesses. The Rust function that the script generates is useful for red-knot: builtin modules are special-cased by the Python interpreter when it comes to module resolution, so we also need to special-case them in our module resolver as well.

## Test Plan

- `cargo test -p red_knot_module_resolver`
- `pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-05 20:20_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-05 20:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-05 20:20_

---

_@carljm approved on 2024-08-05 20:32_

Nice!

---

_Merged by @AlexWaygood on 2024-08-05 20:33_

---

_Closed by @AlexWaygood on 2024-08-05 20:33_

---

_Branch deleted on 2024-08-05 20:33_

---

_Comment by @github-actions[bot] on 2024-08-05 20:38_

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
