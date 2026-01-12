```yaml
number: 13597
title: Allow users to provide custom diagnostic messages when unwrapping calls
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2024-10-01T18:56:24Z
updated_at: 2024-10-01T21:31:36Z
url: https://github.com/astral-sh/ruff/pull/13597
synced_at: 2026-01-12T15:55:44Z
```

# Allow users to provide custom diagnostic messages when unwrapping calls

---

_@charliermarsh_

## Summary

You can now call `return_ty_result` to operate on a `Result` directly thereby using your own diagnostics, as in:

```rust
return dunder_getitem_method
    .call(self.db, &[slice_ty])
    .return_ty_result(self.db, value.as_ref().into(), self)
    .unwrap_or_else(|err| {
        self.add_diagnostic(
            (&**value).into(),
            "call-non-callable",
            format_args!(
                "Method `__getitem__` is not callable on object of type '{}'.",
                value_ty.display(self.db),
            ),
        );
        err.return_ty()
    });
```


---

_Review requested from @carljm by @charliermarsh on 2024-10-01 18:56_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-01 18:56_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-01 18:56_

---

_Label `red-knot` added by @charliermarsh on 2024-10-01 18:56_

---

_@charliermarsh reviewed on 2024-10-01 18:57_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2620 on 2024-10-01 18:57_

This kind of chaining also lets us avoid monomorphizing `unwrap_with_diagnostic`.

---

_@charliermarsh reviewed on 2024-10-01 18:57_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2624 on 2024-10-01 18:57_

It's possible that we'll want to incorporate more information here from `err`. The nice thing is we _can_ if we want.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1061 on 2024-10-01 19:06_

I think we should just use the `Type` variant for this case. The default diagnostics are the same, and kind of the point is that a fully-not-callable union shouldn't really be treated differently from any other not-callable type.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2620 on 2024-10-01 19:09_

The new pattern looks great to me, but I'm not sure I understand the comment. `unwrap_with_diagnostic` is not generic (except over a lifetime), so what does it mean to monomorphize it?

---

_Comment by @github-actions[bot] on 2024-10-01 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2624 on 2024-10-01 19:11_

In fact I would like to :) I think the diagnostic should include the type of `__getitem__` that we found to not be callable. This could really help someone in debugging why their `__getitem__` isn't callable. I think that type is already available in `err`. I would suggest the phrasing "Method `__getitem__` of type {} is not callable on object of type {}."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2645 on 2024-10-01 19:11_

Same comment as above, let's include the type we inferred for `__class_getitem__`.

---

_@carljm approved on 2024-10-01 19:12_

Lovely, thank you!

---

_@carljm reviewed on 2024-10-01 19:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2624 on 2024-10-01 19:23_

I guess this will require some decisions about how to handle the `UnionElement` and `UnionElements` cases; I think we could just use the full union type and omit the detail of which element(s) were not callable, in this case? Don't have strong feelings though, open to options.

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2620 on 2024-10-01 19:36_

Ah sorry, that was a confusing comment out-of-context. I initially considered having `unwrap_with_diagnostic` accept a callback function to format the diagnostics, which would've _then_ made it generic on that callback. By returning result, I avoided making it generic :)

---

_@charliermarsh reviewed on 2024-10-01 19:37_

---

_@charliermarsh reviewed on 2024-10-01 19:53_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2624 on 2024-10-01 19:53_

Sounds good, I'll play around with it. I want something flexible but not too verbose or onerous.

---

_@charliermarsh reviewed on 2024-10-01 21:18_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2624 on 2024-10-01 21:18_

Alright, I went with printing the full union type for now.

---

_Merged by @charliermarsh on 2024-10-01 21:22_

---

_Closed by @charliermarsh on 2024-10-01 21:22_

---

_Branch deleted on 2024-10-01 21:22_

---
