```yaml
number: 12586
title: Upgrade to Rust 1.80
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-1.80
created_at: 2024-07-30T17:43:14Z
updated_at: 2024-07-30T19:32:30Z
url: https://github.com/astral-sh/ruff/pull/12586
synced_at: 2026-01-10T21:47:02Z
```

# Upgrade to Rust 1.80

---

_Pull request opened by @MichaReiser on 2024-07-30 17:43_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Upgrades our development rust version from 1.79 to 1.80

## Test Plan

`cargo test`, `cargo clippy`, `cargo fmt`


---

_Review requested from @carljm by @MichaReiser on 2024-07-30 17:43_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-30 17:43_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-07-30 17:43_

---

_Label `internal` added by @MichaReiser on 2024-07-30 17:43_

---

_Comment by @MichaReiser on 2024-07-30 17:49_

Hmm, the msrv failures are annoying. Because it means that we need to silence the clippy warning

---

_Comment by @github-actions[bot] on 2024-07-30 18:23_

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

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/src/statement/clause.rs`:20 on 2024-07-30 18:35_

```suggestion
///
/// [source](https://docs.python.org/3/reference/compound_stmts.html#compound-statements)
```

---

_@AlexWaygood approved on 2024-07-30 18:36_

---

_Review comment by @dhruvmanila on `Cargo.toml`:159 on 2024-07-30 18:49_

Nice, this will make the fuzzer build output less noisy as for MacOS it always required to use the nightly compiler

---

_@dhruvmanila approved on 2024-07-30 18:49_

---

_@carljm approved on 2024-07-30 19:05_

---

_Merged by @MichaReiser on 2024-07-30 19:18_

---

_Closed by @MichaReiser on 2024-07-30 19:18_

---

_Branch deleted on 2024-07-30 19:18_

---
