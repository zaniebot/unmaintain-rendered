```yaml
number: 12654
title: "[red-knot] Add basic WASM API"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-wasm
created_at: 2024-08-03T18:46:20Z
updated_at: 2024-08-06T07:28:03Z
url: https://github.com/astral-sh/ruff/pull/12654
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] Add basic WASM API

---

_@MichaReiser_

## Summary

Adds a basic WASM API and sets up CI. The WASM build has the added advantage that we'll catch 32bit issues as part of CI.

Future additions that we can add in later PRs

* API to retrieve the type by a given offset
* tracing integration


## Test Plan

Added nodejs test


---

_@MichaReiser reviewed on 2024-08-03 18:47_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:679 on 2024-08-03 18:47_

This is a bit annoying but re-implementing a memory file system just because of time also felt overkill.

---

_Label `red-knot` added by @MichaReiser on 2024-08-03 18:48_

---

_Comment by @github-actions[bot] on 2024-08-03 19:07_

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

_Comment by @codspeed-hq[bot] on 2024-08-03 21:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/red-knot-wasm)

### Merging #12654 will **improve performances by 12.49%**

<sub>Comparing <code>red-knot-wasm</code> (e8cf043) with <code>main</code> (f0318ff)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `red-knot-wasm` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 921.4 µs | 819.1 µs | +12.49% |


---

_@MichaReiser reviewed on 2024-08-05 07:08_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/build.rs`:33 on 2024-08-05 07:08_

We use `deflate` for wasm because building `zstd` for 32bit requires clang which is a rather annoying dependency

---

_Marked ready for review by @MichaReiser on 2024-08-05 07:09_

---

_Review requested from @carljm by @MichaReiser on 2024-08-05 07:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-05 07:09_

---

_Review comment by @carljm on `crates/red_knot_module_resolver/build.rs`:25 on 2024-08-05 20:52_

How does this work when compiling under wasm? Doesn't this mean the following statement will be eliminated under wasm, which would just mean the variable `method` becomes undefined?

---

_@carljm approved on 2024-08-05 20:57_

---

_@MichaReiser reviewed on 2024-08-06 05:47_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/build.rs`:25 on 2024-08-06 05:47_

Ah yeah. I should remove this. My hope was that I could use regular `cfg` attributes in the build script but this doesn't work because the `target_arch` in the build script is the system's `arch` running the build script.

---

_Closed by @MichaReiser on 2024-08-06 06:56_

---

_Reopened by @MichaReiser on 2024-08-06 06:56_

---

_Merged by @MichaReiser on 2024-08-06 07:21_

---

_Closed by @MichaReiser on 2024-08-06 07:21_

---

_Branch deleted on 2024-08-06 07:21_

---
