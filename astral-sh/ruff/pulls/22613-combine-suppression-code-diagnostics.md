```yaml
number: 22613
title: Combine suppression code diagnostics
type: pull_request
state: open
author: amyreese
labels:
  - rule
  - suppression
assignees: []
base: main
head: amy/suppression-grouping
created_at: 2026-01-16T00:34:42Z
updated_at: 2026-01-16T18:50:53Z
url: https://github.com/astral-sh/ruff/pull/22613
synced_at: 2026-01-16T19:01:47Z
```

# Combine suppression code diagnostics

---

_@amyreese_

# Summary

- Report a single combined UnusedNOQA diagnostic when any range suppression
  has one or more unused, disabled, or duplicate lint codes.
- Report a single combined InvalidRuleCode diagnostic for any range suppression
  with one or more invalid lint codes.

# Test Plan

Updated fixtures and snapshots

Fixes #22429, #21873


---

_Comment by @astral-sh-bot[bot] on 2026-01-16 00:44_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `rule` added by @amyreese on 2026-01-16 00:50_

---

_Label `suppression` added by @amyreese on 2026-01-16 00:50_

---

_Review requested from @MichaReiser by @amyreese on 2026-01-16 00:50_

---

_Review requested from @ntBre by @amyreese on 2026-01-16 00:50_

---

_@MichaReiser reviewed on 2026-01-16 08:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:462 on 2026-01-16 08:24_

Nice

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 08:28_

The suppression range here is a bit misleading because it suggests that all codes are unused. I think I would either:

* Highlight the entire suppression diagnostic. That makes it clear that something's up with the suppression and not with an individual code
* Keep underlining the codes, but only group adjacent codes (in which case we'd emit two diagnostics in this case). I slightly prefer this as it gives more precise ranges. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:235 on 2026-01-16 08:34_

Is my understanding correct that the suppressions in `self.valid` are sorted by their `TextRange`? If so, you could then avoid using a hash map and instead use a single `Option<(TextRange, SuppressionDiagnostic)>` that you "flush" when the `TextRange` no longer matches `suppression.comments.disable_comment().range` (and when you're done processing all `self.valid` 

---

_@MichaReiser approved on 2026-01-16 08:36_

---

_@amyreese reviewed on 2026-01-16 16:08_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:235 on 2026-01-16 16:08_

I don't think they're actually sorted at the moment, because outer scopes get matched after inner scopes, but suppression objects from the same range should always end up in the list next to each other.

---

_@MichaReiser reviewed on 2026-01-16 16:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 16:10_

Don't bother with this if this matches what we do for noqa today.

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:246 on 2026-01-16 16:30_

small nit: I think this is the same as the variable above:


```suggestion
                entry.invalid_codes.push(code_str);
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:266 on 2026-01-16 16:33_

nit: could we use [`count`](https://doc.rust-lang.org/nightly/core/iter/trait.Iterator.html#method.count) here?


```suggestion
                        .filter(|code| *code == code_str)
                        .count()
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:299 on 2026-01-16 16:35_

I think `join` will handle this automatically:


```suggestion
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:287 on 2026-01-16 16:41_

It looks like this doesn't actually help here because of how `group` is used, but in principle you can use `into_values` to avoid cloning sometimes.

---

_@amyreese reviewed on 2026-01-16 16:42_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 16:42_

It looks like noqa highlights the entire noqa comment:

```
RUF100 [*] Unused `noqa` directive (unused: `F401`, `E741`)
 --> foo.py:2:12
  |
1 | def f():
2 |     x = 1  # noqa: F401, F841, E741
  |            ^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Remove unused `noqa` directive
1 | def f():
  -     x = 1  # noqa: F401, F841, E741
2 +     x = 1  # noqa: F841
```

---

_@MichaReiser reviewed on 2026-01-16 16:43_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 16:43_

I would then suggest aligning with noqa (which I think should be relatively easy?)

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:321 on 2026-01-16 16:47_

This is probably better suited for a follow-up, and I thought I saw some previous discussion of this, but do these need to be `String`s? I tried making `UnusedCodes::disabled` a `Vec<&'a str>` and it seems to be working locally. Combining that with `into_values` makes this a little nicer:


```suggestion
                            disabled: group.disabled_codes
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/suppression.rs`:427 on 2026-01-16 16:51_

This might be getting too clever, but I think we could go ahead and do the `join` here. The empty string will also work for the `is_empty` check, and then we can avoid allocating a separate `Vec` first.


```suggestion
                .join(", ");
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 16:53_

Yeah I liked the idea in https://github.com/astral-sh/ruff/issues/21873#issuecomment-3634167989 of extending the range to only the adjacent codes (if I understood correctly), but aligning with noqa makes sense too.

---

_@ntBre approved on 2026-01-16 16:54_

Nice!

---

_@amyreese reviewed on 2026-01-16 17:17_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__range_suppressions.snap`:416 on 2026-01-16 17:17_

I've updated the highlighting logic so that:
- if only one code exists or only one is being removed, it highlights that one code
- otherwise, it highlights the entire suppression

I think this matches noqa now.

---

_@amyreese reviewed on 2026-01-16 18:50_

---

_Review comment by @amyreese on `crates/ruff_linter/src/suppression.rs`:321 on 2026-01-16 18:50_

I didn't like this either, but since it would also affect the noqa system I'd prefer to put that change in a different PR

---
