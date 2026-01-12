```yaml
number: 15905
title: "[`refurb`] Also report non-name expressions (`FURB169`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: FURB169
created_at: 2025-02-03T06:15:19Z
updated_at: 2025-02-05T08:10:19Z
url: https://github.com/astral-sh/ruff/pull/15905
synced_at: 2026-01-12T15:55:53Z
```

# [`refurb`] Also report non-name expressions (`FURB169`)

---

_@InSyncWithFoo_

## Summary

Follow-up to #15779.

Prior to this change, non-name expressions are not reported at all:

```python
type(a.b) is type(None)  # no error
```

This change enhances the rule so that such cases are also reported in preview. Additionally:

* The fix will now be marked as unsafe if there are any comments within its range.
* Error messages are slightly modified.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-03 06:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @AlexWaygood on 2025-02-03 11:36_

---

_@MichaReiser reviewed on 2025-02-03 11:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/isinstance_type_none.rs`:72 on 2025-02-03 11:40_

Nit: IDE's can show the argument names inline

```suggestion
    let fix = replace_with_identity_check(expr, call.range, false, checker);
```

---

_Comment by @AlexWaygood on 2025-02-03 11:50_

> None of the aforementioned changes are preview-gated, as they are considered bugfixes/non-breaking, similar to that of #15779.

We discussed this internally and, while it's quite borderline, we think this unfortunately does need to be gated behind preview. It's similar to #15579, and it's true that #15579 increased the scope a little bit, but it felt like #15579 was mainly a bugfix (expanding the scope of the rule was sort-of incidental). The line here is a bit fuzzy, and it's true that this _could_ also be considered a bugfix of sorts. However, Ruff arguably isn't doing anything _wrong_ here right now (it could just be doing a bit better). To us this feels like it's a significant enough expansion of the rule that it should be a preview-only change first.

---

_Comment by @InSyncWithFoo on 2025-02-03 16:52_

> [...] this feels like it's a significant enough expansion of the rule that it should be a preview-only change first.

Preview-gated the first change. The other two are fine, I suppose?

---

_Comment by @AlexWaygood on 2025-02-03 16:59_

> > [...] this feels like it's a significant enough expansion of the rule that it should be a preview-only change first.
> 
> Preview-gated the first change. The other two are fine, I suppose?

yup! Thanks!

---

_@AlexWaygood reviewed on 2025-02-03 18:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/type_none_comparison.rs`:86 on 2025-02-03 18:18_

```suggestion
        && !matches!(other, Expr::Name(_) | Expr::NoneLiteral(_))
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/helpers.rs`:46 on 2025-02-04 10:20_

Is my understanding correct that this was mainly moved from `isinstance_type_none` with the exception that you added the comments check?

---

_@MichaReiser reviewed on 2025-02-04 10:20_

---

_@MichaReiser reviewed on 2025-02-04 10:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB169.py`:40 on 2025-02-04 10:25_

It doesn't seem those get reported in the snapshot. Would you mind taking a look?



---

_@InSyncWithFoo reviewed on 2025-02-05 00:30_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/helpers.rs`:46 on 2025-02-05 00:30_

It was the `negate` parameter/`let op` block that were added, but, yes, that's correct.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview__FURB169_FURB169.py.snap`:326 on 2025-02-05 07:28_

Is it correct that we remove the `type` around the `a := 1` here? same for the next diagnostic. Shouldn't this be `type(a := 1) is None`?

---

_@MichaReiser reviewed on 2025-02-05 07:28_

---

_@InSyncWithFoo reviewed on 2025-02-05 07:33_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview__FURB169_FURB169.py.snap`:326 on 2025-02-05 07:33_

`type(a) is type(None)` is true if and only if `a` is `None`. This is the case with existing tests as well. To be pedantic, `type(a) == type(None)` isn't entirely safe due to operator overloading, but it is reported and fixed the same regardless.

---

_@MichaReiser reviewed on 2025-02-05 07:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview__FURB169_FURB169.py.snap`:326 on 2025-02-05 07:45_

oh dang sorry, I got confused what the rule does. 

---

_@MichaReiser approved on 2025-02-05 07:46_

---

_Label `preview` added by @MichaReiser on 2025-02-05 07:46_

---

_Merged by @MichaReiser on 2025-02-05 07:46_

---

_Closed by @MichaReiser on 2025-02-05 07:46_

---

_Branch deleted on 2025-02-05 08:10_

---
