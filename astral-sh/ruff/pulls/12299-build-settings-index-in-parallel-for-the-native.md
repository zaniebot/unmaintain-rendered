```yaml
number: 12299
title: Build settings index in parallel for the native server
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/parallel-walk-builder
created_at: 2024-07-12T12:10:42Z
updated_at: 2024-07-15T10:11:59Z
url: https://github.com/astral-sh/ruff/pull/12299
synced_at: 2026-01-12T15:55:40Z
```

# Build settings index in parallel for the native server

---

_@dhruvmanila_

## Summary

This PR updates the server to build the settings index in parallel using similar logic as `python_files_in_path`.

This should help with https://github.com/astral-sh/ruff/issues/11366 but ideally we would want to build it lazily.

## Test Plan

`cargo insta test`


---

_Label `server` added by @dhruvmanila on 2024-07-12 12:10_

---

_Comment by @github-actions[bot] on 2024-07-12 12:29_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/.gpt_action_getting_started.ipynb:11:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_bigquery.ipynb:13:1:1: Expected an expression
```

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index/ruff_settings.rs`:130 on 2024-07-12 12:49_

Do we need to respect the `respect_gitignore` setting here?

---

_@MichaReiser approved on 2024-07-12 12:50_

---

_Comment by @MichaReiser on 2024-07-12 12:50_

What's also important to note is that this will exclude directories that are ignored

---

_Comment by @charliermarsh on 2024-07-12 12:56_

Nice.

---

_@dhruvmanila reviewed on 2024-07-12 12:58_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:130 on 2024-07-12 12:58_

I think so. Let me check.

---

_Comment by @dhruvmanila on 2024-07-15 09:52_

> What's also important to note is that this will exclude directories that are ignored

I don't think that's the case if there's no config file present in the root directory. This is because the first loop extracts the settings from the root config file:

https://github.com/astral-sh/ruff/blob/ecd6865d2800a574baf122583b6e463605af2ef5/crates/ruff_server/src/session/index/ruff_settings.rs#L105-L128

This is then used when walking through the project files to determine whether a directory should be excluded or not:

https://github.com/astral-sh/ruff/blob/ecd6865d2800a574baf122583b6e463605af2ef5/crates/ruff_server/src/session/index/ruff_settings.rs#L141-L205

Maybe we can use the fallback settings if there is none in the root directory? https://github.com/astral-sh/ruff/blob/ecd6865d2800a574baf122583b6e463605af2ef5/crates/ruff_server/src/session/index/ruff_settings.rs#L205

---

_@dhruvmanila reviewed on 2024-07-15 09:52_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index/ruff_settings.rs`:130 on 2024-07-15 09:52_

I've added it where by default it's `true` and if we find any config file it'll use the value from that instead.

---

_Merged by @dhruvmanila on 2024-07-15 09:57_

---

_Closed by @dhruvmanila on 2024-07-15 09:57_

---

_Branch deleted on 2024-07-15 09:57_

---
