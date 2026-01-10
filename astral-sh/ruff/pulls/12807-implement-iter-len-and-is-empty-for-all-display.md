```yaml
number: 12807
title: "Implement `iter()`, `len()` and `is_empty()` for all display-literal AST nodes"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/display-len
created_at: 2024-08-11T21:30:35Z
updated_at: 2024-08-12T16:51:00Z
url: https://github.com/astral-sh/ruff/pull/12807
synced_at: 2026-01-10T21:38:32Z
```

# Implement `iter()`, `len()` and `is_empty()` for all display-literal AST nodes

---

_Pull request opened by @AlexWaygood on 2024-08-11 21:30_

## Summary

This PR adds `iter()`, `len()` and `is_empty()` methods to all the AST nodes representing ["display literals"](https://docs.python.org/3/reference/expressions.html#displays-for-lists-sets-and-dictionaries):

- `ExprList`
- `ExprSet`
- `ExprTuple`
- `ExprDict`

Each of these nodes wraps an inner `Vec` of elements; it's rare to use these nodes in linter rules without either iterating over the elements or querying the number of elements. Exposing these methods on the outer nodes makes a lot of our code simpler and more elegant.

## Test Plan

`cargo test`


---

_Label `internal` added by @AlexWaygood on 2024-08-11 21:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-11 21:30_

---

_Comment by @codspeed-hq[bot] on 2024-08-11 21:36_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:alex/display-len)

### Merging #12807 will **not alter performance**

<sub>Comparing <code>AlexWaygood:alex/display-len</code> (23b601e) with <code>main</code> (a99a458)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-11 21:49_

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

_Comment by @dhruvmanila on 2024-08-12 05:04_

> Each of these nodes wraps an inner `Vec` of elements; it's rare to use these nodes in linter rules without either iterating over the elements or querying the number of elements. Exposing these methods on the outer nodes makes a lot of our code simpler and more elegant.

If it makes sense, we could just implement `Deref` for these nodes that returns the inner `Vec` which would automatically expose all these methods (and other methods).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:156 on 2024-08-12 06:40_

I'm not so sure if changing from` map_or` to `map_or_else` is worth here, considering that the computation is fairly cheap (it subtracts -1). A function call would be n-times more expensive than the computation we do here. 

---

_@MichaReiser approved on 2024-08-12 06:40_

---

_Comment by @AlexWaygood on 2024-08-12 10:18_

> If it makes sense, we could just implement `Deref` for these nodes that returns the inner `Vec` which would automatically expose all these methods (and other methods).

Yeah, it's an interesting idea. I'm not sure if methods like `dict.first()` and `dict.last()` make as much sense as being able to iterate over the `ExprDict` node and directly query its length, though. I think I prefer to only implement a selection of methods that we know make sense.

I considered creating a `DisplayLiteral` trait for all of these nodes to implement, but in the end I concluded that it just created more complexity and didn't really simplify anything

---

_@AlexWaygood reviewed on 2024-08-12 10:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:156 on 2024-08-12 10:19_

Yeah, debatable I guess. I'll revert this change.

---

_Merged by @AlexWaygood on 2024-08-12 10:39_

---

_Closed by @AlexWaygood on 2024-08-12 10:39_

---

_Branch deleted on 2024-08-12 16:51_

---
