```yaml
number: 18735
title: "[`ruff`] Frozen Dataclass default should be valid (`RUF009`)"
type: pull_request
state: merged
author: lubaskinc0de
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/frozen-dc-default
created_at: 2025-06-17T19:49:05Z
updated_at: 2025-06-23T19:15:32Z
url: https://github.com/astral-sh/ruff/pull/18735
synced_at: 2026-01-10T18:39:08Z
```

# [`ruff`] Frozen Dataclass default should be valid (`RUF009`)

---

_Pull request opened by @lubaskinc0de on 2025-06-17 19:49_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #17424
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Renamed from "RUF009 - Frozen Dataclass default should be valid #17424" to "[ruff] Frozen Dataclass default should be valid #17424" by @lubaskinc0de on 2025-06-17 20:10_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:109 on 2025-06-18 18:33_

These lines could be simplified to:

```rust
let Some(Stmt::ClassDef(class_def)) = binding.statement(semantic) else {
	return false;
};
```

---

_@InSyncWithFoo reviewed on 2025-06-18 18:33_

---

_@lubaskinc0de reviewed on 2025-06-18 21:14_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:109 on 2025-06-18 21:14_

done

---

_Marked ready for review by @lubaskinc0de on 2025-06-18 21:31_

---

_Converted to draft by @lubaskinc0de on 2025-06-18 21:31_

---

_Marked ready for review by @lubaskinc0de on 2025-06-18 21:32_

---

_Comment by @lubaskinc0de on 2025-06-18 22:12_

ready for review

---

_Comment by @lubaskinc0de on 2025-06-19 09:48_

i am also fixed dataclass_kind helper more info here https://discord.com/channels/1039017663004942429/1039017663512449056/1385177991319126117

---

_Renamed from "[ruff] Frozen Dataclass default should be valid #17424" to "[ruff] Frozen Dataclass default should be valid" by @MichaReiser on 2025-06-19 11:04_

---

_Comment by @github-actions[bot] on 2025-06-20 07:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5ed8704525c03241eec93b19e3e65b0b691269e3/reflex/event.py#L573'>reflex/event.py:573:72:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/5ed8704525c03241eec93b19e3e65b0b691269e3/reflex/event.py#L573'>reflex/event.py:573:72:</a> RUF100 [*] Unused `noqa` directive (unused: `RUF009`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @ntBre on 2025-06-23 14:20_

---

_Label `rule` added by @ntBre on 2025-06-23 14:20_

---

_Renamed from "[ruff] Frozen Dataclass default should be valid" to "[`ruff`] Frozen Dataclass default should be valid (`RUF009`)" by @ntBre on 2025-06-23 14:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/helpers.rs`:168 on 2025-06-23 16:36_

nit: It looks like we only use this function with `is_some_and`. Would it make more sense just to return a `bool` directly? I see that we wouldn't be able to use `?`, but that could just become:

```rust
let Some(qualified_name) = ... else { return false };
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/helpers.rs`:192 on 2025-06-23 16:39_

I think we could just use `arguments.find_keyword("frozen")` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/helpers.rs`:198 on 2025-06-23 16:41_

I think you could use `Truthiness::into_bool` here, with `unwrap_or_default()` if we decide to make the function as a whole return a `bool` instead of an `Option`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:90 on 2025-06-23 17:12_

We should already have access to the `class_def` and there's an existing call to `dataclass_kind` in `function_call_in_dataclass_default`. Could we use those instead of adding this helper? I think that would allow us to remove this function and just inline

```rust
is_frozen_dataclass(dataclass_decorator, semantic)
```

where this function is called on line 161.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:131 on 2025-06-23 17:16_

nit: I'd prefer just leaving these blank lines here.

---

_@ntBre reviewed on 2025-06-23 17:16_

Thanks, this looks great overall! I just have a few suggestions for simplifying things a bit.

---

_@lubaskinc0de reviewed on 2025-06-23 18:20_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:90 on 2025-06-23 18:20_

Sorry, I didn't understand a bit.
So we have
```rust
pub(super) fn is_frozen_dataclass_instantiation(func: &Expr, semantic: &SemanticModel) -> bool {
    semantic.lookup_attribute(func).is_some_and(|id| {
        let binding = &semantic.binding(id);
        let Some(Stmt::ClassDef(class_def)) = binding.statement(semantic) else {
            return false;
        };

        let Some((_, dataclass_decorator)) = dataclass_kind(class_def, semantic) else {
            return false;
        };
        is_frozen_dataclass(dataclass_decorator, semantic)
    })
}
```
This function takes as input a func, which is actually something like (as far as I remember)
```py
@dataclasses.dataclass(frozen=True)
class D:
    d: C = C()  # C() is a func
```
The whole point of the problem is to make it clear to ruff that if func is a call to the frozen dataclass constructor, then it should not be considered an error

As I understand it, you are suggesting to use ``class_def`` and ``dataclass_decorator`` from the ``function_call_in_dataclass_default`` function

But in this context, in the python example above, it will all belong to class D rather than class C, but we need to check that class C objects are immutable. ``is_frozen_dataclass_instantiation`` function does a lookup of the ``func`` attribute and checks that ``func`` (in the example above ``C``) is a definition of frozen dataclass

---

_Review requested from @ntBre by @lubaskinc0de on 2025-06-23 18:24_

---

_@ntBre reviewed on 2025-06-23 18:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:90 on 2025-06-23 18:50_

Ohhh, I see what you mean. I was just confused. Thanks for clarifying! This makes sense then.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:81 on 2025-06-23 18:54_

One last tiny nit here: could you move this below `function_call_in_dataclass_default`? We like to keep the main function for the rule implementation near the top of the file. I think you could also drop `pub(super)` here since it's only used in this file, as far as I could tell.

---

_@ntBre approved on 2025-06-23 18:55_

Thank you, this is great! Just one more tiny nit, and it looks like clippy also has a minor complaint, but this is otherwise good to go.

---

_Review requested from @ntBre by @lubaskinc0de on 2025-06-23 19:05_

---

_@lubaskinc0de reviewed on 2025-06-23 19:06_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/ruff/rules/function_call_in_dataclass_default.rs`:81 on 2025-06-23 19:06_

done

---

_@ntBre approved on 2025-06-23 19:06_

---

_Merged by @ntBre on 2025-06-23 19:10_

---

_Closed by @ntBre on 2025-06-23 19:10_

---
