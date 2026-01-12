```yaml
number: 20251
title: Add support for sorting diagnostics without a range
type: pull_request
state: closed
author: amyreese
labels:
  - internal
  - diagnostics
assignees: []
base: gh/amyreese/1/base
head: gh/amyreese/1/head
created_at: 2025-09-04T19:46:58Z
updated_at: 2025-11-25T02:00:43Z
url: https://github.com/astral-sh/ruff/pull/20251
synced_at: 2026-01-12T15:56:57Z
```

# Add support for sorting diagnostics without a range

---

_@amyreese_

Stack from [ghstack](https://github.com/ezyang/ghstack) (oldest at bottom):
* #20252
* __->__ #20251

----

- Update ruff ordering compare to work with optional ranges (treating
  them as starting from zero)
- Add a simple from impl to convert std::Path to Span with empty range

---

_Review requested from @carljm by @amyreese on 2025-09-04 19:46_

---

_Review requested from @MichaReiser by @amyreese on 2025-09-04 19:46_

---

_Review requested from @sharkdp by @amyreese on 2025-09-04 19:46_

---

_Review requested from @dcreager by @amyreese on 2025-09-04 19:46_

---

_Review request for @dcreager removed by @ntBre on 2025-09-04 19:51_

---

_Review request for @carljm removed by @ntBre on 2025-09-04 19:51_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-04 19:51_

---

_Review requested from @ntBre by @ntBre on 2025-09-04 19:51_

---

_Label `internal` added by @ntBre on 2025-09-04 19:51_

---

_Label `diagnostics` added by @ntBre on 2025-09-04 19:51_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1224 on 2025-09-04 20:48_

Is this used in this PR, and I'm overlooking it? If not, I'd kind of rather group this with the code change that uses it since we're going to land this commit separately.

I'm also not sure this really belongs in `ruff_db`, which is generally agnostic to the `UnifiedFile` variant, while this is specific to a Ruff `SourceFile`. We can probably just inline this code, like we do for IO errors:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ruff/src/diagnostics.rs#L67-L70

I think we can reuse the `Path::to_string_lossy` helper and avoid the `to_owned` call too.

But this looks like the right approach, we don't have access to any other `SourceFile` here 👍 

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:509 on 2025-09-04 21:00_

I think I'd probably just inline `self.range().unwrap_or_default().start()` (and `other...`) and not factor out an internal helper function, but either way is fine.

We should also update the docstring now that this doesn't panic if the span has no range.

More broadly, I'm wondering how helpful this change will be on its own. I checked the references for `Diagnostic::expect_range`, and there are quite a few more than I expected (sorry!). Do you think we should try to remove all of those at once? Otherwise it seems like we could potentially run into problems  beyond this sort call.

Most of the uses are in tests, so it's safe to slap an `unwrap()` on those, but a few of them will need more consideration.

---

_@ntBre reviewed on 2025-09-04 21:11_

---

_Closed by @amyreese on 2025-09-05 00:05_

---

_Branch deleted on 2025-11-25 02:00_

---
