```yaml
number: 13315
title: "[red-knot] Add a convenience method for constructing a union from a list of elements"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/union-from-elements
created_at: 2024-09-10T18:39:47Z
updated_at: 2024-09-10T21:41:58Z
url: https://github.com/astral-sh/ruff/pull/13315
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add a convenience method for constructing a union from a list of elements

---

_Pull request opened by @AlexWaygood on 2024-09-10 18:39_

## Summary

This pattern is coming up a lot, and it will keep coming up! This PR adds a `UnionType::from_elements` method that is somewhat more ergonomic than having to manually call `UnionBuilder::new(&db).add(elem0).add(elem1).build()` every time we want to create a new union.

(PR is stacked on top of #13314)

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-09-10 18:39_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-10 18:39_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-10 18:39_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:631 on 2024-09-10 18:52_

Nit: I would probably call the method `from_iter`

---

_Comment by @github-actions[bot] on 2024-09-10 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-10 18:58_

This is great. It's slightly annoying that most call sites still need the `into_iter().copied`. 

I'm not quit sure if it works but we could consider implementing `From<&Type<'db>> for Type<'db>` and then change the `from_elements` signature to `where I: IntoIterator<Item=T>, T: Into<Type<'db>`


---

_@AlexWaygood reviewed on 2024-09-10 20:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:631 on 2024-09-10 20:21_

I prefer `from_elements()` :( but it looks like the prevailing opinion may be against me here...

---

_@carljm approved on 2024-09-10 21:38_

This is fantastic, thank you!!

---

_Merged by @AlexWaygood on 2024-09-10 21:38_

---

_Closed by @AlexWaygood on 2024-09-10 21:38_

---

_Branch deleted on 2024-09-10 21:38_

---

_@carljm reviewed on 2024-09-10 21:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:631 on 2024-09-10 21:41_

Unfortunately I don't have a strong opinion here to settle this debate :) Both names seem fine to me. I guess I will also come down slightly on the side of `from_iter`, because I think it is obvious that the iterable would have to be an iterable of union elements (what else could it be?), whereas it's less obvious that `from_elements` would take an iterable (though tbh I think this is pretty obvious too, thus my lack of strong opinion...)

I would be curious what the reason is for @MichaReiser and @dhruvmanila preference.

---
