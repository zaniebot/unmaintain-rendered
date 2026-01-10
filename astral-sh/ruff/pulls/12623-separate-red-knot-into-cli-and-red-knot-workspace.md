```yaml
number: 12623
title: "Separate `red_knot` into CLI and `red_knot_workspace` crates"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/red-knot-lsp-0
created_at: 2024-08-02T09:38:36Z
updated_at: 2024-08-02T11:38:42Z
url: https://github.com/astral-sh/ruff/pull/12623
synced_at: 2026-01-10T21:47:02Z
```

# Separate `red_knot` into CLI and `red_knot_workspace` crates

---

_Pull request opened by @dhruvmanila on 2024-08-02 09:38_

## Summary

This PR separates the current `red_knot` crate into two crates:
1. `red_knot` - This will be similar to the `ruff` crate, it'll act as the CLI crate
2. `red_knot_workspace` - This includes everything except for the CLI functionality from the existing `red_knot` crate

Note that the code related to the file watcher is in `red_knot_workspace` for now but might be required to extract it out in the future.

The main motivation for this change is so that we can have a `red_knot server` command. This makes it easier to test the server out without making any changes in the VS Code extension. All we need is to specify the `red_knot` executable path in `ruff.path` extension setting.

~**Question:** Currently, the integration tests are still kept in the `red_knot` crate but I'm happy to move them into the `red_knot_workspace` crate if that's a better place for them to be in.~

## Test Plan

- `cargo build`
- `cargo clippy --workspace --all-targets --all-features`
- `cargo shear --fix`


---

_Label `red-knot` added by @dhruvmanila on 2024-08-02 09:38_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-02 09:38_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-02 09:38_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-02 09:38_

---

_Comment by @github-actions[bot] on 2024-08-02 09:57_

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

_Review comment by @MichaReiser on `crates/red_knot/tests/check.rs`:3 on 2024-08-02 10:14_

I think we could move this test to `red_knot_workspace`. It isn't CLI specific (arguably, it could even be in ruff python semantic). 

---

_@MichaReiser approved on 2024-08-02 10:18_

Nice! 

We may want to move the `watch/*` modules into the CLI but we can wait with doing that until we know what file watching strategy to use in the LSP. I learned that typescrit now uses the VS code watcher:

* https://github.com/microsoft/vscode/issues/222098#issuecomment-2264544964
* https://github.com/microsoft/vscode/issues/222098#issuecomment-2258123813

---

_Merged by @dhruvmanila on 2024-08-02 11:24_

---

_Closed by @dhruvmanila on 2024-08-02 11:24_

---

_Branch deleted on 2024-08-02 11:24_

---
