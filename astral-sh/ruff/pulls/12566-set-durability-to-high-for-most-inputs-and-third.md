```yaml
number: 12566
title: "Set Durability to 'HIGH' for most inputs and third-party libraries"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: set-input-durability
created_at: 2024-07-29T13:49:21Z
updated_at: 2024-07-30T09:18:32Z
url: https://github.com/astral-sh/ruff/pull/12566
synced_at: 2026-01-10T21:47:02Z
```

# Set Durability to 'HIGH' for most inputs and third-party libraries

---

_Pull request opened by @MichaReiser on 2024-07-29 13:49_

## Summary

Salsa supports inputs with different durability levels. Inputs with lower durability are more likely to change. Using higher durability levels allows Salsa to short-circuit the query validation if a query only depends on inputs with high durability and no high durability input has changed. 

* `Files`: Files from third-party dependencies or the vendored file system have a high durability 
* `FileRoot`s have a high durability
* `Workspace`, `Package` have MEDIUM durability
* `Program` has a high durability

This PR only mitigates the cost for validating derived queries for unchanged inputs. The underlying problem that validating the *green* queries is "expensive" remains.

Fixes https://github.com/astral-sh/ruff/issues/12323

## `Files` changes

I noticed that we should only call salsa setters if the input value has changed. We otherwise unnecessarily invalidate all queries depending on that input. However, fixing this revealed that the module resolver relied on this incorrect behavior. 

This PR fixes the module resolver to not use `system().is_directory` which by-passes the salsa invalidation. I instead changed `system_path_to_file` from returning an `Option` to returning a `Result`. The `is_directory` check can then be written by asserting that `system_path_to_file` returns `Err(IsADirectory)`. 

## Testing

The Salsa API currently doesn't expose enough information to automatically verify whether a query validation is short-circuited or not. But I take the codspeed improvement and that all tests still pass as verification enough.

---

_Label `red-knot` added by @MichaReiser on 2024-07-29 13:51_

---

_Comment by @codspeed-hq[bot] on 2024-07-29 13:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/set-input-durability)

### Merging #12566 will **not alter performance**

<sub>Comparing <code>set-input-durability</code> (6894611) with <code>set-input-durability</code> (20fad6c)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Comment by @MichaReiser on 2024-07-29 13:56_

That's exactly the improvement I wanted to see!

---

_Comment by @charliermarsh on 2024-07-29 14:02_

Lol wow

---

_@MichaReiser reviewed on 2024-07-29 14:07_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:322 on 2024-07-29 14:07_

It's somewhat unexpected that Salsa downgrades the durability when setting a new value. I proposed changing the behavior in Salsa but I don't want to block this PR because of it.

---

_Comment by @github-actions[bot] on 2024-07-29 14:10_

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

_@MichaReiser reviewed on 2024-07-29 16:12_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/memory_fs.rs`:212 on 2024-07-29 16:12_

Another bug haha! the memory file system never updated the last modified timestamp and the invalidation only worked because salsa never compared if the input actually changed

---

_Marked ready for review by @MichaReiser on 2024-07-29 16:26_

---

_Review requested from @carljm by @MichaReiser on 2024-07-29 16:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-29 16:26_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files.rs`:29 on 2024-07-29 16:35_

Should `vendored_path_to_file()` also return a `Result`, for symmetry? Unlike `system_path_to_file`, we don't have a need right now to distinguish directories from files for vendored files... but the asymmetry between the two functions is a _little_ odd now, IMO

---

_@AlexWaygood approved on 2024-07-29 16:45_

Nice! This LGTM and I like the change to return a `Result` from `system_path_to_file`

---

_Review comment by @carljm on `crates/red_knot/src/workspace.rs`:333 on 2024-07-29 16:51_

Is this a correctness requirement or just an optimization?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/path.rs`:414 on 2024-07-29 16:56_

It is now possible here that we silence an `IsADirectory` error and claim it is a `NoVersionsFile` error -- I guess maybe technically we can argue it's not wrong (if `VERSIONS` is a directory then there is "no versions file") -- but it's potentially confusing.

---

_@carljm approved on 2024-07-29 17:06_

Looks great! Awesome benchmark results.

---

_@AlexWaygood reviewed on 2024-07-29 17:08_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/path.rs`:414 on 2024-07-29 17:08_

I suppose we could take the opportunity here to give a custom error message if `stdlib/VERSIONS` is a directory rather than a file, now that we have that information with this refactor

---

_@MichaReiser reviewed on 2024-07-29 17:13_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:29 on 2024-07-29 17:13_

I take another look tomorrow. Overall it feels overkill to return the same type only to get symmetry. Especially because it may also require quiet a bit of boilerplate (or we return more errors than are possible). 

---

_@MichaReiser reviewed on 2024-07-29 17:14_

---

_Review comment by @MichaReiser on `crates/red_knot/src/workspace.rs`:333 on 2024-07-29 17:14_

It's an optimization. It avoids bumping the salsa revision and it avoids that `PackageFiles` is marked as changed.

---

_Merged by @MichaReiser on 2024-07-30 09:03_

---

_Closed by @MichaReiser on 2024-07-30 09:03_

---

_Branch deleted on 2024-07-30 09:04_

---
