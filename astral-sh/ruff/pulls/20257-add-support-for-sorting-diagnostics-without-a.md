```yaml
number: 20257
title: Add support for sorting diagnostics without a range
type: pull_request
state: merged
author: amyreese
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: amy/diagnostics-sorting
created_at: 2025-09-05T00:06:30Z
updated_at: 2025-09-05T22:23:32Z
url: https://github.com/astral-sh/ruff/pull/20257
synced_at: 2026-01-10T17:46:21Z
```

# Add support for sorting diagnostics without a range

---

_Pull request opened by @amyreese on 2025-09-05 00:06_

- Update ruff ordering compare to work with optional ranges (treating
  them as starting from zero)


---

_Review requested from @carljm by @amyreese on 2025-09-05 00:06_

---

_Review requested from @MichaReiser by @amyreese on 2025-09-05 00:06_

---

_Review requested from @sharkdp by @amyreese on 2025-09-05 00:06_

---

_Review requested from @dcreager by @amyreese on 2025-09-05 00:06_

---

_Comment by @amyreese on 2025-09-05 18:43_

Updated docstring. @ntBre am I correct that there aren't any unit tests covering the ordering of diagnostics where I could add test cases? Is that worth taking the time to add?

---

_Review requested from @ntBre by @amyreese on 2025-09-05 18:43_

---

_Comment by @ntBre on 2025-09-05 18:54_

It looks like `test_contents` calls `ruff_start_ordering`, so I'm pretty sure all of our snapshot tests actually exercise this behavior, at least the range part since they're mostly single-file tests:

https://github.com/astral-sh/ruff/blob/16d674e63264acdf640349c81ec6263e813a3234/crates/ruff_linter/src/test.rs#L405

I'm guessing some of our CLI tests probably have multiple files and thus use the file part too.

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:509 on 2025-09-05 18:55_

I think we could avoid the `map` calls if we just `unwrap` the range first:


```rust
            self.range().unwrap_or_default().start(),
```

Alternatively, we can probably just drop the `unwrap_or_default`. I think `None` will sort below `Some(...)`, just like unwrapping to a `start` of zero.

```rust
            self.range().map(|range| range.start()),
```

---

_@ntBre approved on 2025-09-05 19:04_

Thanks! I thought of two more ideas for very, very slight simplifications. Feel free to apply one of them or not.

---

_Label `internal` added by @ntBre on 2025-09-05 19:04_

---

_Label `diagnostics` added by @ntBre on 2025-09-05 19:04_

---

_Merged by @amyreese on 2025-09-05 22:23_

---

_Closed by @amyreese on 2025-09-05 22:23_

---

_Branch deleted on 2025-09-05 22:23_

---
