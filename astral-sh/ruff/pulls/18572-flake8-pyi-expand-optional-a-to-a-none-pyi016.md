```yaml
number: 18572
title: "[`flake8-pyi`] Expand `Optional[A]` to `A | None` (`PYI016`)"
type: pull_request
state: merged
author: robsdedude
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/pyi016-optional-none
created_at: 2025-06-08T20:45:21Z
updated_at: 2025-06-29T11:43:19Z
url: https://github.com/astral-sh/ruff/pull/18572
synced_at: 2026-01-12T15:56:21Z
```

# [`flake8-pyi`] Expand `Optional[A]` to `A | None` (`PYI016`)

---

_@robsdedude_

## Summary
Under preview :test_tube: I've expanded rule `PYI016` to also flag type union duplicates containing `None` and `Optional`.

## Test Plan
Examples/tests have been added. I've made sure that the existing examples did not change unless preview is enabled.

## Relevant Issues
 * https://github.com/astral-sh/ruff/issues/18508 (discussing introducing/extending a rule to flag `Optional[None]`)
 * https://github.com/astral-sh/ruff/issues/18546 (where I discussed this addition with @AlexWaygood)



---

_Comment by @github-actions[bot] on 2025-06-08 21:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/0189c50570913d73afe8c854f95dcfd5e7d6dd71/libs/core/langchain_core/language_models/llms.py#L158'>libs/core/langchain_core/language_models/llms.py:158:44:</a> PYI016 [*] Duplicate union member `None`
+ <a href='https://github.com/langchain-ai/langchain/blob/0189c50570913d73afe8c854f95dcfd5e7d6dd71/libs/core/langchain_core/language_models/llms.py#L194'>libs/core/langchain_core/language_models/llms.py:194:44:</a> PYI016 [*] Duplicate union member `None`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI016 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @ntBre by @ntBre on 2025-06-09 20:59_

---

_Marked ready for review by @robsdedude on 2025-06-09 21:03_

---

_Review requested from @AlexWaygood by @robsdedude on 2025-06-09 21:03_

---

_Comment by @AlexWaygood on 2025-06-09 21:05_

Nice, the ecosystem hits are both true positives!

---

_Label `rule` added by @ntBre on 2025-06-17 18:34_

---

_Label `preview` added by @ntBre on 2025-06-17 18:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:98 on 2025-06-17 18:51_

Could we move this check inside the rule itself so we don't have to duplicate it? This feels surprising to me, but I tried removing them locally and they're obviously needed somewhere to avoid duplicate diagnostics, as the comment says.

---

_Review comment by @ntBre on `crates/ruff_linter/src/preview.rs`:138 on 2025-06-17 18:52_

I think the convention here is to prefix these with `is_`. Obviously the previously-last one didn't follow that, but it looks like that has been fixed on `main` too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/mod.rs`:178 on 2025-06-17 18:55_

This is the source of the merge conflict. I think you'll need to resurrect this `preview_rules` function but leave off PYI044.pyi.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:96 on 2025-06-17 19:09_

What do you think about something like this:

```rust
        let diagnostic_range = expr.range();

        let expr = if optional_as_none_in_union_enabled(checker.settings)
            && is_optional_type(checker, expr)
        {
            &VIRTUAL_NONE_LITERAL
        } else {
            expr
        };

        // If we've already seen this union member, raise a violation.
        if seen_nodes.insert(expr.into()) {
            unique_nodes.push(expr);
        } else {
            diagnostics.push(checker.report_diagnostic(
                DuplicateUnionMember {
                    duplicate_name: checker.generator().expr(expr),
                },
                diagnostic_range,
            ));
        }
```

The contents of the two branches looked very similar except for the value of `expr`. The one downside is having to compute the `diagnostic_range` earlier since we can't use the range from `VIRTUAL_NONE_LITERAL`. Another option there is just not shadowing `expr`.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:461 on 2025-06-17 19:15_

I think I'm not as against passing bare `bool` arguments as most people, but if you really want to avoid it, I think the more typical approach is defining a two-variant enum. Maybe something like this:

```rust
enum TraverseOptions {
    Yes,
    No,
}
```

As an even smaller nit here, I think we usually put the docs above the `#[derive]`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview__PYI016_PYI016.py.snap`:935 on 2025-06-17 19:29_

I was a bit skeptical about this transformation in https://github.com/astral-sh/ruff/issues/18508#issuecomment-2950452307 but if you and @AlexWaygood think it's fine, I'm happy with it. I just thought it was likely to be a separate mistake that a safe autofix could hide.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/model.rs`:1628 on 2025-06-17 19:35_

```suggestion
    /// Return `true` if the model is inside an Optional (e.g., the inner `Union` in
    /// `Optional[Union[int, str]]`).
```

I might throw in "directly inside" to make it clear this doesn't keep traversing nested structure. I would have assumed that based on the name alone, but that also applies to the existing function above. Maybe that's just me anyway.

---

_@ntBre approved on 2025-06-17 20:05_

Nice! This is looking good. I just had a few minor suggestions and a question about `Optional[None]`.

---

_@robsdedude reviewed on 2025-06-24 19:53_

---

_Review comment by @robsdedude on `crates/ruff_python_semantic/src/analyze/typing.rs`:461 on 2025-06-24 19:53_

I went with the struct as it's easier to extend should the need for more config options arise in the future. If you'd rather have it an enum, I'm also fine with that. I just find bare bool arguments hard to read at call sites, so I try to avoid them unless the function name gives a strong clue as to what the flag does.

Should I change it anyway?

---

_@robsdedude reviewed on 2025-06-24 19:58_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview__PYI016_PYI016.py.snap`:935 on 2025-06-24 19:58_

This might become hard to track, but how about making the fix unsafe if both
 1. An `Optional` was visited
 2. `None` is (one of) the redundant types in the union

---

_Review comment by @robsdedude on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:98 on 2025-06-24 20:02_

I suppose we could. I was just trying to be consistent with what I saw in the code-base. Just a few lines above there's

```rust
// Avoid duplicate checks if the parent is a union, since these rules already
// traverse nested unions.
if !checker.semantic.in_nested_union() {
```

---

_@robsdedude reviewed on 2025-06-24 20:02_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/typing.rs`:461 on 2025-06-26 20:33_

Are there more options you think might be needed? I guess I was assuming this would be the only one.

In any case, I don't feel too strongly about this, so we can just move ahead with the current version.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__preview__PYI016_PYI016.py.snap`:935 on 2025-06-26 20:36_

I guess it's fine if it would complicate the code, again I don't feel too strongly. And the change is in preview, so we can collect some feedback here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:98 on 2025-06-26 20:37_

Fair enough!

---

_@ntBre approved on 2025-06-26 20:40_

Thanks! Can you resolve the conflicts? 

After that, I can merge after the patch release today.

---

_Renamed from "[`flake8_pyi`] Expand `Optional[A]` to `A | None` in duplicate-union-member (`PYI016`)" to "[`flake8_pyi`] Expand `Optional[A]` to `A | None` in `duplicate-union-member` (`PYI016`)" by @ntBre on 2025-06-27 15:40_

---

_Renamed from "[`flake8_pyi`] Expand `Optional[A]` to `A | None` in `duplicate-union-member` (`PYI016`)" to "[`flake8-pyi`] Expand `Optional[A]` to `A | None` (`PYI016`)" by @ntBre on 2025-06-27 15:40_

---

_Merged by @ntBre on 2025-06-27 15:43_

---

_Closed by @ntBre on 2025-06-27 15:43_

---

_Branch deleted on 2025-06-29 11:43_

---
