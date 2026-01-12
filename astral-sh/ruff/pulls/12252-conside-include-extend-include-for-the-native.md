```yaml
number: 12252
title: "Conside `include`, `extend-include` for the native server"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/server-include
created_at: 2024-07-09T06:15:26Z
updated_at: 2024-07-10T04:27:14Z
url: https://github.com/astral-sh/ruff/pull/12252
synced_at: 2026-01-12T15:55:40Z
```

# Conside `include`, `extend-include` for the native server

---

_@dhruvmanila_

## Summary

This PR updates the native server to consider the `include` and `extend-include` file resolver settings.

fixes: #12242 

## Test Plan

Note: Settings reloading doesn't work for nested configs which is fixed in #12253 so the preview here only showcases root level config.


https://github.com/astral-sh/ruff/assets/67177269/e8969128-c175-4f98-8114-0d692b906cc8



---

_Comment by @github-actions[bot] on 2024-07-09 06:36_

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

_Marked ready for review by @dhruvmanila on 2024-07-09 07:18_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-09 07:18_

---

_Label `bug` added by @dhruvmanila on 2024-07-09 07:32_

---

_Label `server` added by @dhruvmanila on 2024-07-09 07:32_

---

_@MichaReiser reviewed on 2024-07-09 08:25_

Do we also need this for `fix_all`? 

https://github.com/astral-sh/ruff/blob/e6e09ea93a382d86e4dac5e86a030729184c2de8/crates/ruff_server/src/fix.rs#L34-L59

and `format_text_document_range`?

https://github.com/astral-sh/ruff/blob/8338db6c1247a22e0e42ab1b24acc45ae4837149/crates/ruff_server/src/server/api/requests/format_range.rs#L51-L63

Should we have a unified helper to avoid the code repetion?

---

_Comment by @dhruvmanila on 2024-07-09 08:45_

Yes, we'll require for both. I think it'd make sense to add a helper for that.

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server/api/requests/format.rs`:107 on 2024-07-09 13:27_

It seems that we now always have the same block

1. check for exclusion
2. check for inclusion

Can we extract this into a `check_file_included` which does both rather than repeating the two checks in every call site?

---

_@MichaReiser approved on 2024-07-09 13:27_

---

_@dhruvmanila reviewed on 2024-07-10 03:31_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api/requests/format.rs`:107 on 2024-07-10 03:31_

Yeah, I've done so in a new `resolve.rs` file with `is_document_excluded` function.

---

_Merged by @dhruvmanila on 2024-07-10 04:12_

---

_Closed by @dhruvmanila on 2024-07-10 04:12_

---

_Branch deleted on 2024-07-10 04:12_

---
