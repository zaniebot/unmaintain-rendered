```yaml
number: 12842
title: "[red-knot] Add filename and source location for diagnostics"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/diagnostic-info
created_at: 2024-08-12T11:44:35Z
updated_at: 2024-08-12T16:24:03Z
url: https://github.com/astral-sh/ruff/pull/12842
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Add filename and source location for diagnostics

---

_@dhruvmanila_

## Summary

I'm not sure if this is useful but this is a hacky implementation to add the filename and row / column numbers to the current Red Knot diagnostics.

cc @carljm, thoughts? 


---

_Label `red-knot` added by @dhruvmanila on 2024-08-12 11:44_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-12 11:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-12 11:44_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-12 11:44_

---

_Comment by @codspeed-hq[bot] on 2024-08-12 11:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/diagnostic-info)

### Merging #12842 will **not alter performance**

<sub>Comparing <code>dhruv/diagnostic-info</code> (0955282) with <code>main</code> (5400232)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @dhruvmanila on 2024-08-12 12:34_

I can update the test if this format is good enough. It's a bit difficult to include the range for `lint_override` because we'd need to find out the actual `@override` decorator node.

---

_@MichaReiser approved on 2024-08-12 14:08_

---

_Comment by @MichaReiser on 2024-08-12 14:09_

I think this is fine. But I would probably not spend too much time on `lint_override` because we may want to remove the lint rule anyway.

---

_@carljm approved on 2024-08-12 15:36_

100% agree with Micha: this looks good and useful and worth fixing up the tests and landing, but I wouldn't spend extra time on `lint_override`.

---

_Merged by @dhruvmanila on 2024-08-12 15:56_

---

_Closed by @dhruvmanila on 2024-08-12 15:56_

---

_Branch deleted on 2024-08-12 15:56_

---

_Comment by @github-actions[bot] on 2024-08-12 16:10_

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

_Comment by @AlexWaygood on 2024-08-12 16:24_

It looks like some of these tests might be failing on Windows on `main` due to windows paths having different display representations: https://github.com/astral-sh/ruff/actions/runs/10355375507/job/28662886847?pr=12748

---
