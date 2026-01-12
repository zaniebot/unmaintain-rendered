```yaml
number: 14965
title: "[red-knot] Error out when an mdtest code block is unterminated"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: rk-unterminated-code-block
created_at: 2024-12-14T00:21:54Z
updated_at: 2024-12-14T15:50:51Z
url: https://github.com/astral-sh/ruff/pull/14965
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Error out when an mdtest code block is unterminated

---

_@InSyncWithFoo_

## Summary

Resolves #14934.

## Test Plan

Added a unit test.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-14 00:21_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-14 00:21_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-14 00:21_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-14 00:21_

---

_Renamed from "[red-knot] Error out when a code block is unterminated" to "[red-knot] Error out when an mdtest code block is unterminated" by @carljm on 2024-12-14 00:23_

---

_Label `testing` added by @AlexWaygood on 2024-12-14 00:24_

---

_Label `red-knot` added by @AlexWaygood on 2024-12-14 00:24_

---

_Comment by @github-actions[bot] on 2024-12-14 00:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:339 on 2024-12-14 00:34_

This will always say "at index 0" no matter where in the file the unterminated code block starts, because of the way we use `Cursor` to advance through the source text; we are always matching this regex at the start of a string. (Checked this experimentally.)

I _think_ (not tested) that `self.cursor.token_len()` will give you the offset into the original source text that we've consumed so far, and still let you easily provide this info in the error. If for some reason that doesn't work, I'm also OK with just not providing this data. In practice, I'm not sure how useful it is, because I _think_ if any code block other than the last one is unterminated, you'll just get a situation where the contents between the unterminated code block and the next will get swallowed, and it'll end up reporting a later code block as unterminated, not the one you actually need to fix?

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:689 on 2024-12-14 00:37_

Maybe use an example where the code block doesn't start at position 0 in the source text (i.e. preface the source text with some prose or a header) so this test would have caught the issue with the index reporting.

---

_@carljm reviewed on 2024-12-14 00:37_

---

_Comment by @carljm on 2024-12-14 00:37_

Thank you!

---

_@InSyncWithFoo reviewed on 2024-12-14 02:12_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_test/src/parser.rs`:339 on 2024-12-14 02:12_

I changed it so that it emits row indexes instead. I don't feel satisfied just yet, though; how about IDE-inspectable absolute paths with `:row:column` affix?

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:353 on 2024-12-14 05:19_

All errors are already output with the full path as a prefix, so as you have it this ends up rendering as:

```
Error parsing `/Users/carlmeyer/projects/ruff/crates/red_knot_python_semantic/resources/mdtest/scopes/unbound.md`: Unterminated code block at: /Users/carlmeyer/projects/ruff/crates/red_knot_python_semantic/resources/mdtest/scopes/unbound.md:47:0
```

Which seems a bit redundant :)

```suggestion
                "Unterminated code block starting on line {row}",
```

---

_@carljm reviewed on 2024-12-14 05:19_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:34 on 2024-12-14 05:19_

Here's where we already show the path in every error.

---

_@carljm reviewed on 2024-12-14 05:20_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_test/src/lib.rs`:34 on 2024-12-14 05:44_

I was thinking of changing this upstream error message too, but that's probably too much. Reverted to "row" commit.

---

_@InSyncWithFoo reviewed on 2024-12-14 05:44_

---

_@carljm reviewed on 2024-12-14 05:51_

---

_Review comment by @carljm on `crates/red_knot_test/src/lib.rs`:34 on 2024-12-14 05:51_

Looks good, to keep this PR focused. Future PR to further improve error output here is welcome!

---

_@carljm approved on 2024-12-14 05:51_

---

_Merged by @carljm on 2024-12-14 05:51_

---

_Closed by @carljm on 2024-12-14 05:51_

---

_Branch deleted on 2024-12-14 06:21_

---

_Comment by @AlexWaygood on 2024-12-14 15:50_

Thanks @InSyncWithFoo! :D

---
