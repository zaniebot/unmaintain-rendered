```yaml
number: 14825
title: "Support `type[a.X]` with qualified class names"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/qualified-type-of
created_at: 2024-12-06T19:03:16Z
updated_at: 2024-12-06T22:14:52Z
url: https://github.com/astral-sh/ruff/pull/14825
synced_at: 2026-01-12T15:55:49Z
```

# Support `type[a.X]` with qualified class names

---

_@dcreager_

This adds support for `type[a.X]`, where the `type` special form is applied to a qualified name that resolves to a class literal.  This works for both nested classes and classes imported from another module.

Closes #14545 

---

_Review requested from @carljm by @dcreager on 2024-12-06 19:03_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-06 19:03_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-06 19:03_

---

_Review requested from @sharkdp by @dcreager on 2024-12-06 19:03_

---

_@dcreager reviewed on 2024-12-06 19:06_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:79 on 2024-12-06 19:06_

I left this as a TODO because I think this is not specific to `type[]`.  The `unresolved-attribute` error I get is:

> Type `<module 'System("/src/a/b.py")'>` has no attribute `b`

which suggests that our import logic is binding `a` as `a.b` instead of `a`.  Am I interpreting that right?

---

_Label `red-knot` added by @dylwil3 on 2024-12-06 19:08_

---

_Comment by @github-actions[bot] on 2024-12-06 19:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:79 on 2024-12-06 20:01_

Yes, it looks like you are! I think this is a previously-not-known bug, unless @AlexWaygood was already aware of it. Want to file an issue (labeled `red-knot`)? Could also be an interesting one for you to look into, would get you some exposure to the module resolver, which I don't think any of the other tasks do.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4563 on 2024-12-06 20:03_

we can remove "attributes" from this TODO

---

_@carljm approved on 2024-12-06 20:05_

Nice! Looks like this one would have been a better pick for first task 😆 

---

_@AlexWaygood reviewed on 2024-12-06 20:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:79 on 2024-12-06 20:07_

I think I already knew about it, and I _think_ we already have a TODO about it... whether we have an issue is another question... it's late here, so I won't  look into whether we do or don't this evening :-)

---

_@dcreager reviewed on 2024-12-06 22:07_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_of/basic.md`:79 on 2024-12-06 22:07_

Opened https://github.com/astral-sh/ruff/issues/14826, can close it as a dup if it turns out we have another one already open

---

_@dcreager reviewed on 2024-12-06 22:07_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4563 on 2024-12-06 22:07_

Done

---

_Merged by @dcreager on 2024-12-06 22:14_

---

_Closed by @dcreager on 2024-12-06 22:14_

---

_Branch deleted on 2024-12-06 22:14_

---
