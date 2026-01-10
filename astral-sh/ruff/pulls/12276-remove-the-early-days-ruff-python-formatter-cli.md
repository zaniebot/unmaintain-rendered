```yaml
number: 12276
title: Remove the early days ruff_python_formatter CLI
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
base: main
head: delete-python-formatter-bin
created_at: 2024-07-10T12:48:38Z
updated_at: 2024-08-12T07:52:25Z
url: https://github.com/astral-sh/ruff/pull/12276
synced_at: 2026-01-10T21:38:31Z
```

# Remove the early days ruff_python_formatter CLI

---

_Pull request opened by @MichaReiser on 2024-07-10 12:48_

## Summary
This PR deletes the `ruff_python_formatter/src/main.rs` that we used for testing in the very early days. 

This reduces the number of build targets by one.

## Test Plan

`cargo build`


---

_Label `internal` added by @MichaReiser on 2024-07-10 12:48_

---

_Review requested from @konstin by @MichaReiser on 2024-07-10 12:49_

---

_Comment by @github-actions[bot] on 2024-07-10 12:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
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

_Comment by @MichaReiser on 2024-07-10 13:09_

@konstin still uses it. Let's keep it for now (even if it means having a dependency on clap :( )

---

_Closed by @MichaReiser on 2024-07-10 13:09_

---

_Branch deleted on 2024-08-12 07:52_

---
