```yaml
number: 15521
title: "[`refurb`] Preserve comments (`FURB171`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - fixes
assignees: []
base: main
head: FURB171
created_at: 2025-01-16T00:13:23Z
updated_at: 2025-01-30T17:20:26Z
url: https://github.com/astral-sh/ruff/pull/15521
synced_at: 2026-01-12T15:55:51Z
```

# [`refurb`] Preserve comments (`FURB171`)

---

_@InSyncWithFoo_

## Summary

Resolves #10063.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-16 00:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-16 07:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-16 07:09_

I'm not sure if we should add more token based fixes. They add a fair amount of complexity. 

---

_@InSyncWithFoo reviewed on 2025-01-16 12:54_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-16 12:54_

I'm unaware of a better solution (how else can I collect the comments from a range?). What do you suggest?

---

_@MichaReiser reviewed on 2025-01-16 13:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-16 13:25_

I haven't looked at the fix in detail but my general preference is to accept the removal of comments and instead mark the fix as unsafe if it removes a comment. 

---

_@InSyncWithFoo reviewed on 2025-01-16 13:31_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-16 13:31_

This wouldn't be resolving #10063, then. Might as well close it.

---

_@dhruvmanila reviewed on 2025-01-17 05:56_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-17 05:56_

There's some discussion around comment handling in fixes here: https://github.com/astral-sh/ruff/issues/9790

---

_@MichaReiser reviewed on 2025-01-17 09:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/single_item_membership_test.rs`:191 on 2025-01-17 09:04_

That's fair, although there's a reference to the discussion #9790 that wasn't resolved back then. 

I'm still leaning towards making the fix as unsafe if there's a comment over implementing all this token handling. While impressive, it's just a lot of complexity, especially if we add it to every rule. 

---

_Label `fixes` added by @MichaReiser on 2025-01-17 09:04_

---

_Comment by @dylwil3 on 2025-01-30 14:06_

> I'm still leaning towards making the fix as unsafe if there's a comment over implementing all this token handling. While impressive, it's just a lot of complexity, especially if we add it to every rule.

I agree with this take. I think eventually we should develop a more holistic approach to handling comments, and for now prefer to mark as unsafe (unless the comment handling is very straightforward). 

Gonna close this for now, but thank you for the implementation! Maybe it can inspire the future, general approach to comment handling!

---

_Closed by @dylwil3 on 2025-01-30 14:06_

---

_Branch deleted on 2025-01-30 17:20_

---
