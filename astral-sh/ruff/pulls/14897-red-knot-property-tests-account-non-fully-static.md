```yaml
number: 14897
title: "[red-knot] Property tests: account non-fully-static types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/property-test-fixes
created_at: 2024-12-10T19:39:03Z
updated_at: 2024-12-10T19:57:45Z
url: https://github.com/astral-sh/ruff/pull/14897
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Property tests: account non-fully-static types

---

_Pull request opened by @sharkdp on 2024-12-10 19:39_

## Summary

I probably broke these tests myself in #14758. I don't know how that happened because I even added a new property test there, but maybe I ran the tests with a filter. Anyway, adding a `is_fully_static` premise to these two tests seems like the right thing to do.

There is another (actual) problem with `assignable_to_is_reflexive` which we'll also need to address, I'll open a ticket.

## Test Plan

```
cargo test -p red_knot_python_semantic -- --ignored types::property_tests::stable
```


---

_Label `red-knot` added by @sharkdp on 2024-12-10 19:39_

---

_Review requested from @carljm by @sharkdp on 2024-12-10 19:39_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-10 19:39_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-10 19:39_

---

_@carljm approved on 2024-12-10 19:46_

Looks good, thank you!

I guess we could also add a `non_fully_static_types_do_not_participate_in_equivalence` test?

---

_Comment by @github-actions[bot] on 2024-12-10 19:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2024-12-10 19:55_

---

_Closed by @sharkdp on 2024-12-10 19:55_

---

_Branch deleted on 2024-12-10 19:55_

---
