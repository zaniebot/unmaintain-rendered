```yaml
number: 12711
title: Update salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: upgrade-salsa
created_at: 2024-08-06T07:25:54Z
updated_at: 2025-11-21T20:38:03Z
url: https://github.com/astral-sh/ruff/pull/12711
synced_at: 2026-01-10T16:48:01Z
```

# Update salsa

---

_Pull request opened by @MichaReiser on 2024-08-06 07:25_

## Summary

Pull in the latest salsa changes. The most notable changes are:

* The `Handle<Database>` was removed again. This requires re-adding `snapshot` methods to `RootDatabase` and using an `Arc` for System. @dhruvmanila this requires [some trickery](https://salsa.zulipchat.com/#narrow/stream/333573-salsa-3.2E0/topic/Expose.20an.20API.20to.20cancel.20other.20queries) to implement `system_mut`. I can with that in case my PR lands first
* Fewer thread local storage usage (let's see if this shows in the benchmarks
* The builder API is now part of the official salsa

## Test Plan

`cargo test`


---

_Converted to draft by @MichaReiser on 2024-08-06 07:53_

---

_Label `red-knot` added by @MichaReiser on 2024-08-06 07:56_

---

_Comment by @codspeed-hq[bot] on 2024-08-06 08:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/upgrade-salsa)

### Merging #12711 will **improve performances by 6.6%**

<sub>Comparing <code>upgrade-salsa</code> (1b8ef1e) with <code>main</code> (8e6aa78)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `upgrade-salsa` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `red_knot_check_file[incremental]` | 1.7 ms | 1.6 ms | +6.6% |


---

_Comment by @github-actions[bot] on 2024-08-06 08:16_

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

_Marked ready for review by @MichaReiser on 2024-08-06 08:17_

---

_Review requested from @carljm by @MichaReiser on 2024-08-06 08:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-06 08:17_

---

_@AlexWaygood approved on 2024-08-06 08:29_

---

_Comment by @MichaReiser on 2024-08-06 11:10_

I'll wait for @dhruvmanila to merge the LSP first

---

_Comment by @dhruvmanila on 2024-08-06 11:32_

@MichaReiser I've merged the LSP PR

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-08-06 12:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_workspace/src/workspace/files.rs`:60 on 2024-08-06 12:21_

I think you might want to update the comment to reflect the change from `runtime_mut`

---

_@dhruvmanila approved on 2024-08-06 12:22_

---

_Merged by @MichaReiser on 2024-08-06 13:17_

---

_Closed by @MichaReiser on 2024-08-06 13:17_

---

_Branch deleted on 2024-08-06 13:17_

---

_@axelkar reviewed on 2025-11-21 20:38_

---

_Review comment by @axelkar on `crates/ruff_db/src/files.rs`:275 on 2025-11-21 20:38_

There's no "SAFETY:" comment here. I'm wondering if it's safe to do this in my project utilizing Salsa. Is there anything I should watch out for?

---
