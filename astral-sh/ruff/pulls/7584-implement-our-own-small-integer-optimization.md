```yaml
number: 7584
title: Implement our own small-integer optimization
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lex
created_at: 2023-09-21T21:56:54Z
updated_at: 2023-09-25T15:21:40Z
url: https://github.com/astral-sh/ruff/pull/7584
synced_at: 2026-01-12T15:55:24Z
```

# Implement our own small-integer optimization

---

_@charliermarsh_

## Summary

This is a follow-up to #7469 that attempts to achieve similar gains, but without introducing malachite. Instead, this PR removes the `BigInt` type altogether, instead opting for a simple enum that allows us to store small integers directly and only allocate for values greater than `i64`:

```rust
/// A Python integer literal. Represents both small (fits in an `i64`) and large integers.
#[derive(Clone, PartialEq, Eq, Hash)]
pub struct Int(Number);

#[derive(Debug, Clone, PartialEq, Eq, Hash)]
pub enum Number {
    /// A "small" number that can be represented as an `i64`.
    Small(i64),
    /// A "large" number that cannot be represented as an `i64`.
    Big(Box<str>),
}

impl std::fmt::Display for Number {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        match self {
            Number::Small(value) => write!(f, "{value}"),
            Number::Big(value) => write!(f, "{value}"),
        }
    }
}
```

We typically don't care about numbers greater than `isize` -- our only uses are comparisons against small constants (like `1`, `2`, `3`, etc.), so there's no real loss of information, except in one or two rules where we're now a little more conservative (with the worst-case being that we don't flag, e.g., an `itertools.pairwise` that uses an extremely large value for the slice start constant). For simplicity, a few diagnostics now show a dedicated message when they see integers that are out of the supported range (e.g., `outdated-version-block`).

An additional benefit here is that we get to remove a few dependencies, especially `num-bigint`.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2023-09-21 22:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/lex)

### Merging #7584 will **improve performances by 8.58%**

<sub>Comparing <code>charlie/lex</code> (afeb2c7) with <code>main</code> (65aebf1)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/lex` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[numpy/globals.py]` | 233.8 µs | 228.6 µs | +2.26% |
| ⚡ | `lexer[large/dataset.py]` | 9.8 ms | 9 ms | +8.58% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 621.3 µs | 592 µs | +4.96% |
| ⚡ | `lexer[pydantic/types.py]` | 4.1 ms | 4 ms | +3.98% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 2 ms | 1.9 ms | +2.91% |


---

_Comment by @github-actions[bot] on 2023-09-21 22:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-21 22:55_

---

_Review requested from @konstin by @charliermarsh on 2023-09-21 22:55_

---

_Marked ready for review by @charliermarsh on 2023-09-21 22:55_

---

_@charliermarsh reviewed on 2023-09-21 22:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2589 on 2023-09-21 22:55_

The choice of `isize` is semi-arbitrary, but the reasoning is laid out above. We could even use `i64`, it shouldn't matter much.

---

_@charliermarsh reviewed on 2023-09-22 03:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:119 on 2023-09-22 03:14_

So this truncates to `isize` (that is, we don't flag bad file permissions that exceed `isize`). Which seems not ideal. But it actually looks like an improvement, since before, we only flagged file permissions that could be cast to `u16`?

---

_@dhruvmanila reviewed on 2023-09-22 03:46_

---

_Review comment by @dhruvmanila on `foo.py`:1 on 2023-09-22 03:46_

I think this got added accidentally?

---

_@charliermarsh reviewed on 2023-09-22 03:57_

---

_Review comment by @charliermarsh on `foo.py`:1 on 2023-09-22 03:57_

Thanks!

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_2020/rules/compare.rs`:238 on 2023-09-22 05:45_

I'd implement `PartialEq<isize>` instead:
```rust
#[derive(PartialEq)]
enum Int {
    Small(isize),
    Large(Box<str>)
}

impl PartialEq<isize> for Int {
    fn eq(&self, other: &isize) -> bool {
        self == &Int::Small(*other)
    }
}
```

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:119 on 2023-09-22 05:52_

yeah the before one was incorrect. Could we handle this with something like `is_small_and(FnOnce(isize) -> bool)` and `is_large_or(FnOnce(isize) -> bool)`? Alternatively emit the diagnostic on an else branch on `if let Some(mask) = int_value(mode_arg, checker.semantic())`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2589 on 2023-09-22 06:10_

I think we should use `i64` here and not `isize`. The size types are intended for pointer-like numbers: The length of an array or string. This use case is different because we don't want to represent a length but an absolute number. 

Not having to think about whether its an `i64` or `i32` depending on the target platform will make our life easier.

---

_Review comment by @konstin on `crates/ruff_python_ast/src/nodes.rs`:2586 on 2023-09-22 06:13_

We should use `Box<str>` since the value is never mutated and the size will be 2 pointers instead of 3.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2581 on 2023-09-22 06:13_

I like the new abstraction but I think we should hide its internal to make it easier to change the internals in the future (e.g. using tagged pointers). 

* Make `Int` a `struct` with one inner field
* Create a private enum that represents the two different states
* Implement `Eq`, `PartialEq` for all types for which it is safe (`i8..i64`, `u8..u32`).
* Implement `as_*` helpers that either return `T` if the conversion is safe or `Option<T>` if the value falls into the `> i64` range. 
* Implement `Debug` to hide the internal representation (should always serialize to the value)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:376 on 2023-09-22 06:15_

Can we add a test for a very long sequence of `0`: `000000000000000000000000000000000000000000000000000....` to ensure it gets catched by line 355?

---

_@konstin approved on 2023-09-22 06:15_

this is neat

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:119 on 2023-09-22 06:18_

Shouldn't all non `i64` values be reported as a bad file permissions? They're certainly wrong. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:377 on 2023-09-22 06:21_

I would remove this warning message as it is not helpful to users. They do not indicate where the unsupported version number appears in their source. It ends up being one of those warnings that you can't get rid of and end up wasting a ton of time on.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:108 on 2023-09-22 06:23_

What's preventing us from matching on `Int` directly and create a diagnostic if the value is too large (because the `CmpOp::GtE` likely fails? 

Which brings up a good point. We should implement `PartialOrd` and `Ord` for all types for which we can safely support it.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:354 on 2023-09-22 06:24_

Let us implement `FromStr` (or is it Parse?) on the `Int` type. 

---

_@MichaReiser requested changes on 2023-09-22 06:25_

I really like the direction this is going and it seems sufficient for our use case. 

But I think we should abstract away our internal representation to allow changing it in the future and using `i64` instead of `isize` is important for consistent results across architectures.

---

_@charliermarsh reviewed on 2023-09-22 19:46_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:108 on 2023-09-22 19:46_

I don't think you can implement `Ord` for mixed types, only `PartialOrd`?

---

_@charliermarsh reviewed on 2023-09-22 19:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:108 on 2023-09-22 19:48_

Also, I don't think `CmpOp::GtE` would be likely to fail, since Python does support numbers larger than this.

---

_@charliermarsh reviewed on 2023-09-22 20:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bandit/rules/bad_file_permissions.rs`:119 on 2023-09-22 20:36_

I added a diagnostic for this.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-22 21:08_

---

_Comment by @charliermarsh on 2023-09-22 21:09_

Okay, I think this contains all of the desired feedback.

---

_@charliermarsh reviewed on 2023-09-22 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2625 on 2023-09-22 21:14_

These are sort of random but they allow you to pattern match against `0` and `1`, which end up being the only values that we pattern-match against.

---

_@charliermarsh reviewed on 2023-09-22 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2635 on 2023-09-22 21:14_

These are private, we could just remove them and call the constructor directly in the `parse` methods.

---

_@charliermarsh reviewed on 2023-09-22 21:15_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/nodes.rs`:2805 on 2023-09-22 21:15_

@MichaReiser - I don't know if you had anything fancier than this in mind. I just did `From`, `as_*`, and `PartialEq` for `u8`, `u16`, `u32`, `i8`, `i16`, `i32`, and `i64`.

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:133 on 2023-09-23 08:12_

i really like this!

---

_@konstin reviewed on 2023-09-23 08:12_

---

_@konstin reviewed on 2023-09-23 08:12_

---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_bandit/rules/snmp_insecure_version.rs`:56 on 2023-09-23 08:12_

that's a neat trick

---

_Label `internal` added by @MichaReiser on 2023-09-25 07:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2581 on 2023-09-25 07:25_

Nit: make `Number` private (it's an implementation detail

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2630 on 2023-09-25 07:27_

Nit: You get `parse` for free if you implement `FromStr`. You can then call `let num: Int = str.parse()`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2623 on 2023-09-25 07:28_

Nit: May be worth to move the type into its own `int.rs`. I didn't expect it in `nodes.rs` (e.g. it's also used by tokens)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2698 on 2023-09-25 07:29_

```suggestion
    pub const fn as_i64(&self) -> Option<i64> {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2805 on 2023-09-25 07:30_

Nope. It's very boring code but it improves the API. Thank you

---

_@MichaReiser approved on 2023-09-25 07:30_

---

_@dhruvmanila approved on 2023-09-25 07:41_

Nice!

---

_Merged by @charliermarsh on 2023-09-25 15:13_

---

_Closed by @charliermarsh on 2023-09-25 15:13_

---

_Branch deleted on 2023-09-25 15:13_

---
