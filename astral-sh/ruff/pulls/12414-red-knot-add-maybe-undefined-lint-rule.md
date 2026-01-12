```yaml
number: 12414
title: "[red-knot] add maybe-undefined lint rule"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/maybe-undef
created_at: 2024-07-20T00:19:18Z
updated_at: 2024-07-22T20:54:01Z
url: https://github.com/astral-sh/ruff/pull/12414
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] add maybe-undefined lint rule

---

_@carljm_

Add a lint rule to detect if a name is definitely or possibly undefined at a given usage.

If I create the file `undef/main.py` with contents:

```python
x = int
def foo():
    z
    return x
if flag:
    y = x
y
```

And then run `cargo run --bin red_knot -- --current-directory ../ruff-examples/undef`, I get the output:

```
Name 'z' used when not defined.
Name 'flag' used when not defined.
Name 'y' used when possibly not defined.
```

If I modify the file to add `y = 0` at the top, red-knot re-checks it and I get the new output:

```
Name 'z' used when not defined.
Name 'flag' used when not defined.
```

Note that `int` is not flagged, since it's a builtin, and `return x` in the function scope is not flagged, since it refers to the global `x`.

---

_Label `red-knot` added by @carljm on 2024-07-20 00:19_

---

_Review requested from @MichaReiser by @carljm on 2024-07-20 00:19_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-20 00:19_

---

_Comment by @github-actions[bot] on 2024-07-20 00:32_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:246 on 2024-07-20 10:05_

Nit: I would prefer keeping the field private and instead add a `.contains` method to `UnionType` itself. 



---

_@MichaReiser approved on 2024-07-20 10:06_

Nice! It's exciting to see how "easy" complex rules are

Let's write a unit test for this. I don't think it should be too hard with our testing infra.

---

_Merged by @carljm on 2024-07-22 20:54_

---

_Closed by @carljm on 2024-07-22 20:54_

---

_Branch deleted on 2024-07-22 20:54_

---
