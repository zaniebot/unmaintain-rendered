```yaml
number: 13580
title: "Support classes that implement `__call__`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/callable
created_at: 2024-10-01T04:08:50Z
updated_at: 2024-10-01T17:57:18Z
url: https://github.com/astral-sh/ruff/pull/13580
synced_at: 2026-01-12T15:55:44Z
```

# Support classes that implement `__call__`

---

_@charliermarsh_

## Summary

This looked straightforward and removes some TODOs.


---

_Review requested from @carljm by @charliermarsh on 2024-10-01 04:08_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-01 04:08_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-01 04:08_

---

_Label `red-knot` added by @charliermarsh on 2024-10-01 04:08_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:618 on 2024-10-01 04:09_

This is just `Type::Class(_instance_ty)`. Should I do that directly, or do we prefer to go through `to_meta_type`?

---

_@charliermarsh reviewed on 2024-10-01 04:09_

---

_Comment by @github-actions[bot] on 2024-10-01 04:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-10-01 14:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:618 on 2024-10-01 14:19_

I think here you can just do it directly! Maybe add a comment saying that because it's a dunder it's important to access it as an attribute on the class rather than the instance, though (matching the runtime semantics)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:623 on 2024-10-01 14:25_

there are some existing bugs where we don't do this elsewhere (that we should fix), but it's actually somewhat important: because we're accessing `__call__` as an attribute on the class rather than the instance, `__call__` is actually a "pure function" here rather than a method. That means that we need to manually insert `self` into the `arg_types` we're passing into the `.call()` method here.

It doesn't make a difference right now, but it will when we start validating the types of arguments passed into typed functions.

I guess something like this?

```suggestion
                    &dunder_call_method.call(db, std::iter::once(self).chain(arg_types).collect::<Vec<_>>())
```

---

_@AlexWaygood approved on 2024-10-01 14:25_

Nice!

---

_@carljm reviewed on 2024-10-01 14:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:623 on 2024-10-01 14:37_

We may want to better optimize the way we pass around call args in the future to avoid this re-allocation (the runtime has the same issue!) but I think for now it's fine to just get the args right, it'll be easier to see how to optimize it once we have more cases in place, and are actually validating args.

---

_@carljm approved on 2024-10-01 14:38_

Looks great!

---

_@AlexWaygood reviewed on 2024-10-01 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:623 on 2024-10-01 16:50_

> there are some existing bugs where we don't do this elsewhere (that we should fix)

(I fixed the other bugs in #13593!)

---

_@charliermarsh reviewed on 2024-10-01 17:06_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:623 on 2024-10-01 17:06_

Makes sense, sounds good.

---

_@charliermarsh reviewed on 2024-10-01 17:06_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:623 on 2024-10-01 17:06_

Easy to avoid the allocation here later.

---

_Merged by @charliermarsh on 2024-10-01 17:15_

---

_Closed by @charliermarsh on 2024-10-01 17:15_

---

_Branch deleted on 2024-10-01 17:15_

---

_@AlexWaygood reviewed on 2024-10-01 17:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:616 on 2024-10-01 17:19_

Ah I was actually thinking you could get rid of yet another layer of indirection here, i.e.

```suggestion
                let dunder_call_method = class.class_member(db, "__call__");

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:616 on 2024-10-01 17:57_

https://github.com/astral-sh/ruff/pull/13595

---

_@charliermarsh reviewed on 2024-10-01 17:57_

---
