```yaml
number: 14596
title: "[`refurb`] Handle non-finite decimals in `verbose-decimal-constructor (FURB157)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: decimal-nan
created_at: 2024-11-26T02:53:36Z
updated_at: 2024-12-03T00:13:25Z
url: https://github.com/astral-sh/ruff/pull/14596
synced_at: 2026-01-12T15:55:48Z
```

# [`refurb`] Handle non-finite decimals in `verbose-decimal-constructor (FURB157)`

---

_@dylwil3_

This PR extends the Decimal parsing used in [verbose-decimal-constructor (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/) to better handle non-finite `Decimal` objects, avoiding some false negatives.

Closes #14587


---

_Comment by @github-actions[bot] on 2024-11-26 03:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2024-11-26 08:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB157.py`:50 on 2024-11-26 08:27_

I would prefer an explanatory comment here rather than a riddle :laughing: 

I suspect that the importance is the missing `-` sign but the existing comment leaves me wondering if there's more to it.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:159 on 2024-11-26 08:29_

```suggestion
                &normalized_float_string,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:176 on 2024-11-26 08:31_

Unrelated to your fix: To avoid an allocation in many cases, it might be worth trying an exact match first before calling `to_lowercase`.

```rust
let trimmed = float.value.to_str().trim();

if is_inf_infinity_or_nan(trimmed) {
	return;
}

let normalized_float_string = trimmed.to_lowercase();

if is_inf_infinity_or_nan(&normalized_flaot_string) {
	return;
}
```



---

_@MichaReiser approved on 2024-11-26 08:31_

---

_@dylwil3 reviewed on 2024-11-26 16:37_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:176 on 2024-11-26 16:37_

Maybe I'm misunderstanding but I think we have to allocate anyway: we only get to escape early here if `float.value` is _not_ inf, infinity, or nan (after trimming and lowercasing). So if we first checked that the trimmed version wasn't "nan" etc., we would still have to rule out the case where it's "NaN".

---

_@dylwil3 reviewed on 2024-11-26 16:45_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:159 on 2024-11-26 16:45_

actually had to revert that - type mismatch (`matches` doesn't let you compare `&String` and `&str`)

---

_@MichaReiser reviewed on 2024-11-26 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:176 on 2024-11-26 17:01_

Yes, it's just that we do it less often. I guess the overall idea is to spell out all common variations so that the rule can short-circuit for almost all valid-cases and only allocates for uncommon spellings (e.g. `nAn`). 



---

_@AlexWaygood reviewed on 2024-11-27 11:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:159 on 2024-11-27 11:48_

I'm personally fine with `.as_str()`, but in case it's helpful @dylwil3, the way to make this compile would be to do this:

```suggestion
                &*normalized_float_string,
```

What that's saying to Rust is:
1. First dereference the `normalized_float_string` variable (the `*` dereferences things), converting the type from `String` to `str`. (A `String` is a smart pointer, where the pointer points to some underlying data of type `str`).
2. And then borrow it again, converting the type from `str` to `&str`

Something that comes up a lot in Ruff's linter rules is situations where you have a `&Box<Expr>`, but you need an `&Expr` to be able to match on it. One way to get from a `&Box<Expr>` to an `&Expr` is to use `as_ref()`, because there's this convenient implementation for `Box` in the Rust standard library that will take care of the conversion for you: https://doc.rust-lang.org/std/boxed/struct.Box.html#impl-AsRef%3CT%3E-for-Box%3CT,+A%3E. But another way of doing that is to use the invocation `&**var` (where `var` has type `&Box<Expr>`): what that does is:
- Dereference `var` once, converting the type from `&Box<Expr>` to `Box<Expr>`.
- Dereference `var` _again_, converting the type from `Box<Expr>` to `Expr`. (`Box` is a smart pointer, just like `String`, so dereferencing the variable gets you from the pointer to the underlying data that the pointer is pointing to.)
- And then borrow `var`, converting the type from `Expr` to `&Expr`.

The `&*` and `&**` stuff can get some getting used to, but some people prefer it as it's a bit more explicit than some alternative idioms. I think that's more true with the `.as_ref()` situation than the `.as_str()` situation, though -- a `.as_ref()` call could really return anything (you have to lookup the implementation to know exactly), whereas I think it's fairly obvious from an `as_str` call what it's returning.

(Sorry if you already know some of this, just thought it might be helpful! It took me a while to figure some of this out when I was new to Rust.)

---

_@InSyncWithFoo reviewed on 2024-11-29 18:03_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/refurb/FURB157.py`:51 on 2024-11-29 18:03_

Typo: `Deimcal` &rarr; `Decimal`.

---

_@dylwil3 reviewed on 2024-12-01 20:24_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:176 on 2024-12-01 20:24_

Ok, I think the latest commit avoids an allocation for this check in all cases - let me know if you think it's a bit too awkward or if there's a better way 😄 I wanted to avoid using a regex or any other heavy machinery since it seemed a bit overpowered for this case.

---

_@dylwil3 reviewed on 2024-12-03 00:13_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:176 on 2024-12-03 00:13_

(Gonna merge this in to ever-so-slightly ease your code review burden... if you wake up in a cold sweat I'll make a followup PR 😄 )

---

_Merged by @dylwil3 on 2024-12-03 00:13_

---

_Closed by @dylwil3 on 2024-12-03 00:13_

---

_Branch deleted on 2024-12-03 00:13_

---
