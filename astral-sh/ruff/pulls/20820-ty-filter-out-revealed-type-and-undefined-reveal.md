```yaml
number: 20820
title: "[ty] Filter out `revealed-type` and `undefined-reveal` diagnostics from mdtest snapshots"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/suppress-reveal-type-snapshots
created_at: 2025-10-12T15:31:35Z
updated_at: 2025-10-12T18:39:34Z
url: https://github.com/astral-sh/ruff/pull/20820
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Filter out `revealed-type` and `undefined-reveal` diagnostics from mdtest snapshots

---

_@AlexWaygood_

## Summary

As we discussed in ty planning on Friday, `revealed-type` and `undefined-reveal` diagnostics often make mdtest snapshots very noisy. They're almost never relevant to the reason why you've enabled snapshotting for a specific mdtest. The `undefined-reveal` diagnostics can be avoided by explicitly importing `reveal_type` (which is not something you'd usually do in an mdtest), but the `revealed-type` diagnostics can only be avoided by having a separate mdtest entirely that enables snapshotting and avoids having any `reveal_type` calls in it.

This PR suppresses `revealed-type` and `undefined-reveal` diagnostics from the diagnostics snapshotted during an mdtest run. If we do want to capture a snapshot that shows how these diagnostics are displayed, we can still do that as an end-to-end CLI test in the `ty` crate.

## Test Plan

Snapshots!


---

_Review requested from @carljm by @AlexWaygood on 2025-10-12 15:31_

---

_Label `testing` added by @AlexWaygood on 2025-10-12 15:31_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-12 15:31_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-12 15:31_

---

_Label `ty` added by @AlexWaygood on 2025-10-12 15:31_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-12 15:31_

---

_Comment by @github-actions[bot] on 2025-10-12 15:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-12 15:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-10-12 16:15_

I think I'd prefer if we disable the rule instead of filtering out the diagnostics here:

https://github.com/astral-sh/ruff/blob/dc64c086336148c57db14060c86f448351710b2e/crates/ty_test/src/db.rs#L29

We could then (in the future, not in this PR), add a `rule_selection` option to mdtests, to re-enable the rule when necessary

---

_Comment by @AlexWaygood on 2025-10-12 16:50_

> I think I'd prefer if we disable the rule instead of filtering out the diagnostics here:

That would disable these rules for mdtest as a whole, rather than just the snapshots, though, right? There's a fair amount of logic in mdtest that's specifically there so that we don't have to do that. I suggested just suppressing these rules instead in mdtest [just over a year ago](https://github.com/astral-sh/ruff/issues/13708#issuecomment-2406098565), but @carljm was against it, which is why the approach I'm taking here attempts to specifically filter the diagnostics out of mdtest _snapshots_ without disabling these diagnostics for regular mdtest.

---

_@MichaReiser reviewed on 2025-10-12 16:56_

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:386 on 2025-10-12 16:56_

Can't we use `UNDEFINED_REVEAL::name()` here to avoid hardcoding the name

---

_@MichaReiser approved on 2025-10-12 16:57_

---

_@AlexWaygood reviewed on 2025-10-12 17:18_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/lib.rs`:386 on 2025-10-12 17:18_

It requires making `UNDEFINED_REVEAL` `pub` rather than `pub(crate)`, which would make it the first `pub` diagnostic in `types/diagnostic.rs`. But I'm okay with that if you are 😄

---

_@MichaReiser reviewed on 2025-10-12 17:49_

---

_Review comment by @MichaReiser on `crates/ty_test/src/lib.rs`:386 on 2025-10-12 17:49_

I'm fine with that

---

_Merged by @AlexWaygood on 2025-10-12 18:39_

---

_Closed by @AlexWaygood on 2025-10-12 18:39_

---

_Branch deleted on 2025-10-12 18:39_

---
