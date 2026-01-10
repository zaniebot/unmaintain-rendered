```yaml
number: 13182
title: "[ruff_text_size] Make TextRange support Ord/PartialOrd"
type: pull_request
state: closed
author: ndmitchell
labels: []
assignees: []
base: main
head: textrange-ord
created_at: 2024-08-31T08:28:04Z
updated_at: 2024-09-01T16:44:21Z
url: https://github.com/astral-sh/ruff/pull/13182
synced_at: 2026-01-10T21:38:32Z
```

# [ruff_text_size] Make TextRange support Ord/PartialOrd

---

_Pull request opened by @ndmitchell on 2024-08-31 08:28_

## Summary

Add Ord/PartialOrd for TextRange. These were useful for some work I was doing where I wanted to put tokens in a known order. There's only one sensible definition of Ord, so might as well support that.

## Test Plan

Built it with Cargo.

---

_Comment by @github-actions[bot] on 2024-08-31 08:46_

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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
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
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_salesforce.ipynb:14:1:1: Expected an expression
```

</p>
</details>




---

_Review requested from @MichaReiser by @MichaReiser on 2024-09-01 11:04_

---

_Comment by @MichaReiser on 2024-09-01 11:11_

Thanks for creating this PR. 

> There's only one sensible definition of Ord, so might as well support that.

I see at least three possible ordering:

* By length
* By start
* By end position

That's why I would prefer to keep the explicit `TextRange::ordering` method. It also keeps the type closer to `std::ops::Range` which doesn't implement `Ord`.

---

_Closed by @MichaReiser on 2024-09-01 11:11_

---

_Branch deleted on 2024-09-01 16:43_

---

_Comment by @ndmitchell on 2024-09-01 16:44_

Fair enough. I'll newtype wrapper it for my project then (I want to embed it in a type where I really want canonical ordering - but don't care what that ordering is).

---
