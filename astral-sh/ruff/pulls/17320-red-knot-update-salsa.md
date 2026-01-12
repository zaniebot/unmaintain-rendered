```yaml
number: 17320
title: "[red-knot] Update salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa
created_at: 2025-04-09T16:57:40Z
updated_at: 2025-04-09T20:22:04Z
url: https://github.com/astral-sh/ruff/pull/17320
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Update salsa

---

_@MichaReiser_

## Summary

Update Salsa to pull in https://github.com/salsa-rs/salsa/pull/788 which fixes the, by now, famous *access to field whilst the value is being initialized*. 

This PR also re-enables all tests that previously triggered the panic.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-04-09 16:58_

---

_Comment by @github-actions[bot] on 2025-04-09 16:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-04-09 17:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fupdate-salsa)

### Merging #17320 will **not alter performance**

<sub>Comparing <code>micha/update-salsa</code> (09a8741) with <code>main</code> (144484d)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @MichaReiser on 2025-04-09 17:05_

Lol... Codspeed is confused. It thinks I acknowledged this regression a month ago. 



---

_Comment by @github-actions[bot] on 2025-04-09 17:07_

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

_Comment by @MichaReiser on 2025-04-09 17:20_

It looks like the main reason for the regression is that we now see more `deep_verified_memo` calls compared to before. I'm a bit confused about this because the old code should already always have reached the `deep_verify_memo` (unless it panicked). The only thing I can think of is that we now run `deep_verify_memo` for provisional memos where we always returned `Changed` before... but wouldn't that lead to a perf improvement instead of a regression?

---

_Marked ready for review by @MichaReiser on 2025-04-09 17:31_

---

_Review requested from @carljm by @MichaReiser on 2025-04-09 17:31_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-09 17:31_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-09 17:31_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-09 17:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:199 on 2025-04-09 17:32_

You need to delete the big TODO comment immediately prior to this line as well ;)

---

_@AlexWaygood requested changes on 2025-04-09 17:32_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-09 17:34_

---

_@AlexWaygood requested changes on 2025-04-09 17:37_

you missed the language tag on this line, which also needs to be changed to `py`:

https://github.com/astral-sh/ruff/blob/781b653511c920b3ecbdac851a0aa03c3a5f97b1/crates/red_knot_python_semantic/resources/mdtest/properties.md?plain=1#L244

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-09 17:43_

---

_@AlexWaygood approved on 2025-04-09 17:46_

Thanks!

---

_@dcreager approved on 2025-04-09 17:58_

:tada: Thanks Micha!

---

_Comment by @MichaReiser on 2025-04-09 18:07_

I think I may have identified the reason for the perf regression... https://github.com/salsa-rs/salsa/pull/789

---

_Comment by @MichaReiser on 2025-04-09 18:10_

TODO: Update the commit after my other PR is merged

---

_Comment by @MichaReiser on 2025-04-09 18:16_

Yes, this was it. Now it's neutral. 

---

_Comment by @MichaReiser on 2025-04-09 18:22_

I'll merge this tomorrow. The salsa merge queue is too slow. If anyone wants this sooner. Feel free to update the `Cargo.toml`s with the commit hash from the salsa repo (tip of head)

---

_@carljm approved on 2025-04-09 20:21_

---

_Merged by @carljm on 2025-04-09 20:22_

---

_Closed by @carljm on 2025-04-09 20:22_

---

_Branch deleted on 2025-04-09 20:22_

---
