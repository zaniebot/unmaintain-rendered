```yaml
number: 12679
title: Upgrade Salsa to a version with a 32bit compatible concurrent vec
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: upgrade-salsa-32bit-compatible
created_at: 2024-08-05T06:42:18Z
updated_at: 2024-08-05T07:02:28Z
url: https://github.com/astral-sh/ruff/pull/12679
synced_at: 2026-01-12T15:55:41Z
```

# Upgrade Salsa to a version with a 32bit compatible concurrent vec

---

_@MichaReiser_


## Summary

Fixes https://github.com/astral-sh/ruff/issues/12661

## Test Plan
* I tried to build `i686-unknown-linux-gnu` before upgrading and building failed with the reported error message.
* I was able to build for `i686-unknown-linux-gnu` after upgrading salsa (I didn't test if linking succeeds too)


---

_Label `internal` added by @MichaReiser on 2024-08-05 06:44_

---

_Comment by @codspeed-hq[bot] on 2024-08-05 06:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/upgrade-salsa-32bit-compatible)

### Merging #12679 will **improve performances by 17.52%**

<sub>Comparing <code>upgrade-salsa-32bit-compatible</code> (e01d1c7) with <code>main</code> (b647f3f)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `upgrade-salsa-32bit-compatible` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 1.9 ms | 1.7 ms | +17.52% |


---

_Merged by @MichaReiser on 2024-08-05 06:50_

---

_Closed by @MichaReiser on 2024-08-05 06:50_

---

_Branch deleted on 2024-08-05 06:50_

---

_Comment by @github-actions[bot] on 2024-08-05 07:02_

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
