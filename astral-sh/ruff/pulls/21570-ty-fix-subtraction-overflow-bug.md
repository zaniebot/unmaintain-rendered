```yaml
number: 21570
title: "[ty] Fix subtraction overflow bug"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/fix-subtraction
created_at: 2025-11-21T19:05:36Z
updated_at: 2025-11-21T21:34:42Z
url: https://github.com/astral-sh/ruff/pull/21570
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix subtraction overflow bug

---

_Pull request opened by @BurntSushi on 2025-11-21 19:05_

PR #21549 introduced a subtle overflow bug that seemed impossible, but
can empirically happen. This PR fixes it by saturating to zero.

I did try to write a regression test for this, but couldn't manage it.
Instead, I'll attach before-and-after screen recordings.


---

_Review requested from @carljm by @BurntSushi on 2025-11-21 19:05_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-11-21 19:05_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-11-21 19:05_

---

_Review requested from @sharkdp by @BurntSushi on 2025-11-21 19:05_

---

_Review requested from @dcreager by @BurntSushi on 2025-11-21 19:05_

---

_Review request for @dcreager removed by @BurntSushi on 2025-11-21 19:05_

---

_Review request for @carljm removed by @BurntSushi on 2025-11-21 19:05_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-11-21 19:05_

---

_Label `bug` added by @BurntSushi on 2025-11-21 19:05_

---

_Label `server` added by @BurntSushi on 2025-11-21 19:05_

---

_Label `ty` added by @BurntSushi on 2025-11-21 19:06_

---

_Comment by @BurntSushi on 2025-11-21 19:06_

Before:

https://github.com/user-attachments/assets/9cd9bd36-b73a-42a9-9702-a27f6606a5d8

After:

https://github.com/user-attachments/assets/740de241-9ce8-477b-bf0b-46c8be48543a



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 19:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@Gankra approved on 2025-11-21 20:05_

Hmm I wonder if this is getting confused about utf8 vs utf16... 😨 

---

_Merged by @BurntSushi on 2025-11-21 20:07_

---

_Closed by @BurntSushi on 2025-11-21 20:07_

---

_Branch deleted on 2025-11-21 20:07_

---

_Comment by @RasmusNygren on 2025-11-21 20:11_

Oh that was sloppy by me, not sure how I didn't see that in the playground. :thinking: 

This seems to (at the very least) happen when the cursor is in the middle of a token at the start of the file. This seems to reproduce it:
```rust
let builder = completion_test_builder(
            "\
f<CURSOR>oo = 1
",
);
builder.build();
```

---

_Comment by @Gankra on 2025-11-21 20:14_

Oh I see, are we subtracting the size of the whole token from the cursor, rather than token-up-to-the-cursor?

---

_Comment by @RasmusNygren on 2025-11-21 21:34_

We do get the range for the entire token https://github.com/astral-sh/ruff/blob/438ef334d3e71ef0ac70927c799fb159d8f25267/crates/ty_ide/src/completion.rs#L1266

The subtraction is probably a bit off in general then and there might be some other scenarios where we don't suppress suggestions (even though we should) if the cursor is not at the end of the current token. Had a quick check but couldn't figure out a test case that trigger such a scenario even though I feel like they must exist. I'll have a think.


---
