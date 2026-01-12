```yaml
number: 12620
title: "[`tryceratops`] Add `BaseException` to raise-vanilla-class rule (`TRY002`)"
type: pull_request
state: merged
author: epenet
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-02T07:54:40Z
updated_at: 2024-08-05T08:46:23Z
url: https://github.com/astral-sh/ruff/pull/12620
synced_at: 2026-01-12T15:55:41Z
```

# [`tryceratops`] Add `BaseException` to raise-vanilla-class rule (`TRY002`)

---

_@epenet_

Fixes #12612


---

_Comment by @github-actions[bot] on 2024-08-02 08:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @epenet on 2024-08-02 08:33_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-05 07:47_

---

_Label `needs-decision` added by @MichaReiser on 2024-08-05 07:47_

---

_Comment by @MichaReiser on 2024-08-05 07:48_

Code looks good to me. We only have to decide on https://github.com/astral-sh/ruff/issues/12612#issuecomment-2263942167

---

_Comment by @AlexWaygood on 2024-08-05 07:53_

This makes sense to me. It's unfortunate to deviate from `tryceratops` here, but I think it definitely makes sense to include `BaseException` as well as `Exception`.

Should we also include `ExceptionGroup` and `BaseExceptionGroup`, though? (That's a genuine question that I'm not sure the answer to -- I haven't had much opportunity to use these fancy new features yet! It might be that they belong in this rule, or it might be that they deserve their own rule... not sure.)

---

_@AlexWaygood reviewed on 2024-08-05 07:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/tryceratops/rules/raise_vanilla_class.rs`:75 on 2024-08-05 07:56_

This is resolving the qualified name twice when it only needs to resolve it once -- we can micro-optimize this a little bit:

```suggestion
    if checker
        .semantic()
        .resolve_qualified_name(node)
        .is_some_and(|qualified_name| {
            matches!(qualified_name.segments(), ["" | "builtins", "Exception" | "BaseException"])
        })
```

---

_Comment by @AlexWaygood on 2024-08-05 08:44_

Let's land this for now, we can tackle the `ExceptionGroup` and `BaseExceptionGroup` issues as a followup. I think this is a strict improvement.

Thanks @epenet!

---

_Label `needs-decision` removed by @AlexWaygood on 2024-08-05 08:44_

---

_Merged by @AlexWaygood on 2024-08-05 08:45_

---

_Closed by @AlexWaygood on 2024-08-05 08:45_

---

_Branch deleted on 2024-08-05 08:46_

---
