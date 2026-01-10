```yaml
number: 14489
title: Fix unnecessary space around power op in overlong f-string expressions
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/remove-soft-line-conditional-content
created_at: 2024-11-20T14:32:54Z
updated_at: 2024-11-22T12:01:24Z
url: https://github.com/astral-sh/ruff/pull/14489
synced_at: 2026-01-10T20:50:57Z
```

# Fix unnecessary space around power op in overlong f-string expressions

---

_Pull request opened by @MichaReiser on 2024-11-20 14:32_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/14487

The `RemoveSoftLineBuffer` should remove any content "expanded"-only content. For example, it removes all soft line breaks and replaces `soft_line_break_or_space` with a `space`. 
However, it didn't remove any `if_group_breaks` usages. 

This PR extends the `RemoveSoftLineBuffer` to also remove any content inside `if_group_breaks`. This is slightly more complicated because it requires removing multiple elements and
`if_group_breaks` call can be nested.

## Test Plan

Added test


---

_Label `bug` added by @MichaReiser on 2024-11-20 14:33_

---

_Label `formatter` added by @MichaReiser on 2024-11-20 14:33_

---

_Label `preview` added by @MichaReiser on 2024-11-20 14:33_

---

_Marked ready for review by @MichaReiser on 2024-11-20 14:34_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-11-20 14:34_

---

_Comment by @github-actions[bot] on 2024-11-20 15:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @dhruvmanila on `crates/ruff_formatter/src/buffer.rs`:534 on 2024-11-21 06:38_

I don't think the return type of this method is being used anywhere, we should remove it.

---

_Review comment by @dhruvmanila on `crates/ruff_formatter/src/buffer.rs`:466 on 2024-11-21 06:45_

This is just for my understanding - the reason this exists here in addition to being in `should_drop` as well is because of the `Default` state which would mean that the `should_drop` will return `false`. But, we don't need to match against `EndConditionalContent` as it'll be taken care during the `should_drop` check.

I wonder if this could be simplified to only have a counter where any value > 0 indicates that we're in "if_group_breaks" and 0 indicates that we're outside of it. Or, do you foresee any other states that we might want to track in `RemoveSoftLineBreaksState`?

---

_@dhruvmanila reviewed on 2024-11-21 06:51_

---

_@MichaReiser reviewed on 2024-11-21 07:06_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/buffer.rs`:466 on 2024-11-21 07:06_

We could probably just use one `usize` for it where `0` indicates that we're inside an `if_group_breaks` if we change the implementation to start the level at `1` instead of zero. 

However, I do like the explicit state because it documents what's going on here. 

---

_@dhruvmanila reviewed on 2024-11-21 07:30_

---

_Review comment by @dhruvmanila on `crates/ruff_formatter/src/buffer.rs`:466 on 2024-11-21 07:30_

I'm fine with the explicit state, I just found the multiple usages of incrementing but only a single usage of decrementing the value a bit confusing. I had to look at it a bit closely to understand why this is the case.

---

_@MichaReiser reviewed on 2024-11-21 08:15_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/buffer.rs`:466 on 2024-11-21 08:15_

> I just found the multiple usages of incrementing but only a single usage of decrementing the value a bit confusing

I don't think that would change by switching to using an `usize` instead of an explicit enum (we could "hide it" by using `saturating_sub`). 

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/buffer.rs`:466 on 2024-11-21 08:39_

There was actually a bug that we called increment twice in some cases. I refactored the code a bit

---

_@MichaReiser reviewed on 2024-11-21 08:39_

---

_@dhruvmanila approved on 2024-11-21 11:15_

Thanks!

---

_Merged by @MichaReiser on 2024-11-22 12:01_

---

_Closed by @MichaReiser on 2024-11-22 12:01_

---

_Branch deleted on 2024-11-22 12:01_

---
