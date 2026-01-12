```yaml
number: 13206
title: "Avoid allocating `OrderedSet` in `UnionBuilder::simplify`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/union-builder-avoid-set
created_at: 2024-09-02T08:14:45Z
updated_at: 2024-09-02T09:17:03Z
url: https://github.com/astral-sh/ruff/pull/13206
synced_at: 2026-01-12T15:55:43Z
```

# Avoid allocating `OrderedSet` in `UnionBuilder::simplify`

---

_@MichaReiser_


## Summary

Calling `OrderedSet::is_superset` with a `[...].into()` allocates a new `OrderedSet`. The compiler may be able to
figure out that the set isn't strictly necessary, although I doubt that it is (requires reasoning across `is_superset` and `Into`). 

This PR removes the `is_superset` call with two simple `contains` calls. It also tries to preserve the type orders
(which may also help with performance because `remove` in an `OrderedSet` is `O(n)`.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-09-02 08:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-02 08:14_

---

_Label `red-knot` added by @MichaReiser on 2024-09-02 08:14_

---

_Comment by @github-actions[bot] on 2024-09-02 08:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-09-02 08:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/builder.rs`:89 on 2024-09-02 08:52_

nit: we could match against a slice of elements (`as_slice`) to avoid indexing

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/builder.rs`:89 on 2024-09-02 08:56_

Unfortunately not. The `OrderedSet::as_slice` method doesn't return a regular slice. It's a newtype wrapper around `Slice` that doesn't support matching.

---

_@MichaReiser reviewed on 2024-09-02 08:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/builder.rs`:89 on 2024-09-02 09:02_

We don't really need the `shrink_to_fit()` call here unless there's >=2 elements, right?

```suggestion

                match self.elements.len() {
                    0 => Type::Never,
                    1 => self.elements[0],
                    _ => {
                        self.elements.shrink_to_fit();
                        Type::Union(UnionType::new(self.db, self.elements))
                    }
                }
```

---

_@AlexWaygood approved on 2024-09-02 09:02_

---

_Merged by @MichaReiser on 2024-09-02 09:07_

---

_Closed by @MichaReiser on 2024-09-02 09:07_

---

_Branch deleted on 2024-09-02 09:07_

---
