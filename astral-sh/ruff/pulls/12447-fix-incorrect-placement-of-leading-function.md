```yaml
number: 12447
title: Fix incorrect placement of leading function comment with type params
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: leading-function-def-comment-with-type-params
created_at: 2024-07-22T07:42:14Z
updated_at: 2024-07-22T12:17:03Z
url: https://github.com/astral-sh/ruff/pull/12447
synced_at: 2026-01-12T15:55:41Z
```

# Fix incorrect placement of leading function comment with type params

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12430

```py
@decorator
# remark
def foo[S](x: S) -> S: ...
```

was formatted to 

```py
@decorator
def foo# remark
[S](x: S) -> S: ...
```

This PR marks the `# remark` as dangling comment and formats it between the decorator and the function header.

## Test Plan

Added tests. I also added test for classes.


---

_Label `bug` added by @MichaReiser on 2024-07-22 07:42_

---

_Label `formatter` added by @MichaReiser on 2024-07-22 07:42_

---

_Comment by @github-actions[bot] on 2024-07-22 07:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_canvaslms.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:17:1:13: Simple statements must be separated by newlines or semicolons
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_canvaslms.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_outlook.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:17:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_@dhruvmanila approved on 2024-07-22 09:00_

---

_Merged by @MichaReiser on 2024-07-22 12:17_

---

_Closed by @MichaReiser on 2024-07-22 12:17_

---

_Branch deleted on 2024-07-22 12:17_

---
