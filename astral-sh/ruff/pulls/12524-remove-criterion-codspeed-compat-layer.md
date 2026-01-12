```yaml
number: 12524
title: Remove criterion/codspeed compat layer
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: simplify-benchmark-setup
created_at: 2024-07-26T10:08:52Z
updated_at: 2024-08-02T15:50:56Z
url: https://github.com/astral-sh/ruff/pull/12524
synced_at: 2026-01-12T15:55:41Z
```

# Remove criterion/codspeed compat layer

---

_@MichaReiser_

## Summary

The `ruff_benchmark` crate uses a feature to either use `criterion` or `codspeed_criterion_compat`. 
This used to be necessary because running benchmarks locally didn't work with `codspeed_criterion_compat`. This seems to be no longer the case.

This PR removes our own compat layer.

## Test Plan

`cargo bench`


---

_Label `ci` added by @MichaReiser on 2024-07-26 10:12_

---

_Comment by @codspeed-hq[bot] on 2024-07-26 10:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/simplify-benchmark-setup)

### Merging #12524 will **not alter performance**

<sub>Comparing <code>simplify-benchmark-setup</code> (07242e7) with <code>main</code> (9f72f47)</sub>



### Summary

`✅ 30` untouched benchmarks

`🆕 3` new benchmarks
`⁉️ 3 (👁 3)` dropped benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `simplify-benchmark-setup` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `red_knot_check_file[cold]` | N/A | 14 ms | N/A |
| 🆕 | `red_knot_check_file[incremental]` | N/A | 446 µs | N/A |
| 🆕 | `red_knot_check_file[without_parse]` | N/A | 6.5 ms | N/A |
| 👁 | `red_knot_check_file[cold]` | 14.1 ms | N/A | N/A |
| 👁 | `red_knot_check_file[incremental]` | 445.5 µs | N/A | N/A |
| 👁 | `red_knot_check_file[without_parse]` | 6.5 ms | N/A | N/A |


---

_Merged by @MichaReiser on 2024-07-26 10:22_

---

_Closed by @MichaReiser on 2024-07-26 10:22_

---

_Branch deleted on 2024-07-26 10:22_

---

_Comment by @github-actions[bot] on 2024-07-26 10:31_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---
