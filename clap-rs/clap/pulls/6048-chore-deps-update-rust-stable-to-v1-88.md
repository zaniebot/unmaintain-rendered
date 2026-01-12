```yaml
number: 6048
title: "chore(deps): Update Rust Stable to v1.88"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-06-26T19:38:17Z
updated_at: 2025-06-26T19:45:28Z
url: https://github.com/clap-rs/clap/pull/6048
synced_at: 2026-01-12T16:14:17Z
```

# chore(deps): Update Rust Stable to v1.88

---

_@renovate_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.87` -> `1.88` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.88`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1880-2025-06-26)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.87.0...1.88.0)

\==========================

<a id="1.88.0-Language"></a>

## Language

- [Stabilize `#![feature(let_chains)]` in the 2024 edition.](https://redirect.github.com/rust-lang/rust/pull/132833)
  This feature allows `&&`-chaining `let` statements inside `if` and `while`, allowing intermixture with boolean expressions. The patterns inside the `let` sub-expressions can be irrefutable or refutable.
- [Stabilize `#![feature(naked_functions)]`.](https://redirect.github.com/rust-lang/rust/pull/134213)
  Naked functions allow writing functions with no compiler-generated epilogue and prologue, allowing full control over the generated assembly for a particular function.
- [Stabilize `#![feature(cfg_boolean_literals)]`.](https://redirect.github.com/rust-lang/rust/pull/138632)
  This allows using boolean literals as `cfg` predicates, e.g. `#[cfg(true)]` and `#[cfg(false)]`.
- [Fully de-stabilize the `#[bench]` attribute](https://redirect.github.com/rust-lang/rust/pull/134273). Usage of `#[bench]` without `#![feature(custom_test_frameworks)]` already triggered a deny-by-default future-incompatibility lint since Rust 1.77, but will now become a hard error.
- [Add warn-by-default `dangerous_implicit_autorefs` lint against implicit autoref of raw pointer dereference.](https://redirect.github.com/rust-lang/rust/pull/123239)
  The lint [will be bumped to deny-by-default](https://redirect.github.com/rust-lang/rust/pull/141661) in the next version of Rust.
- [Add `invalid_null_arguments` lint to prevent invalid usage of null pointers.](https://redirect.github.com/rust-lang/rust/pull/119220)
  This lint is uplifted from `clippy::invalid_null_ptr_usage`.
- [Change trait impl candidate preference for builtin impls and trivial where-clauses.](https://redirect.github.com/rust-lang/rust/pull/138176)
- [Check types of generic const parameter defaults](https://redirect.github.com/rust-lang/rust/pull/139646)

<a id="1.88.0-Compiler"></a>

## Compiler

- [Stabilize `-Cdwarf-version` for selecting the version of DWARF debug information to generate.](https://redirect.github.com/rust-lang/rust/pull/136926)

<a id="1.88.0-Platform-Support"></a>

## Platform Support

- [Demote `i686-pc-windows-gnu` to Tier 2.](https://blog.rust-lang.org/2025/05/26/demoting-i686-pc-windows-gnu/)

Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.

[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html

<a id="1.88.0-Libraries"></a>

## Libraries

- [Remove backticks from `#[should_panic]` test failure message.](https://redirect.github.com/rust-lang/rust/pull/136160)
- [Guarantee that `[T; N]::from_fn` is generated in order of increasing indices.](https://redirect.github.com/rust-lang/rust/pull/139099), for those passing it a stateful closure.
- [The libtest flag `--nocapture` is deprecated in favor of the more consistent `--no-capture` flag.](https://redirect.github.com/rust-lang/rust/pull/139224)
- [Guarantee that `{float}::NAN` is a quiet NaN.](https://redirect.github.com/rust-lang/rust/pull/139483)

<a id="1.88.0-Stabilized-APIs"></a>

## Stabilized APIs

- [`Cell::update`](https://doc.rust-lang.org/stable/std/cell/struct.Cell.html#method.update)
- [`impl Default for *const T`](https://doc.rust-lang.org/nightly/std/primitive.pointer.html#impl-Default-for-*const+T)
- [`impl Default for *mut T`](https://doc.rust-lang.org/nightly/std/primitive.pointer.html#impl-Default-for-*mut+T)
- [`HashMap::extract_if`](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html#method.extract_if)
- [`HashSet::extract_if`](https://doc.rust-lang.org/stable/std/collections/struct.HashSet.html#method.extract_if)
- [`proc_macro::Span::line`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.line)
- [`proc_macro::Span::column`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.column)
- [`proc_macro::Span::start`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.start)
- [`proc_macro::Span::end`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.end)
- [`proc_macro::Span::file`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.file)
- [`proc_macro::Span::local_file`](https://doc.rust-lang.org/stable/proc_macro/struct.Span.html#method.local_file)

These previously stable APIs are now stable in const contexts:

- [`NonNull<T>::replace`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.replace)
- [`<*mut T>::replace`](https://doc.rust-lang.org/stable/std/primitive.pointer.html#method.replace)
- [`std::ptr::swap_nonoverlapping`](https://redirect.github.com/rust-lang/rust/pull/137280)
- [`Cell::{replace, get, get_mut, from_mut, as_slice_of_cells}`](https://redirect.github.com/rust-lang/rust/pull/137928)

<a id="1.88.0-Cargo"></a>

## Cargo

- [Stabilize automatic garbage collection.](https://redirect.github.com/rust-lang/cargo/pull/14287/)
- [use `zlib-rs` for gzip compression in rust code](https://redirect.github.com/rust-lang/cargo/pull/15417/)

<a id="1.88.0-Rustdoc"></a>

## Rustdoc

- [Doctests can be ignored based on target names using `ignore-*` attributes.](https://redirect.github.com/rust-lang/rust/pull/137096)
- [Stabilize the `--test-runtool` and `--test-runtool-arg` CLI options to specify a program (like qemu) and its arguments to run a doctest.](https://redirect.github.com/rust-lang/rust/pull/137096)

<a id="1.88.0-Compatibility-Notes"></a>

## Compatibility Notes

- [Finish changing the internal representation of pasted tokens](https://redirect.github.com/rust-lang/rust/pull/124141). Certain invalid declarative macros that were previously accepted in obscure circumstances are now correctly rejected by the compiler. Use of a `tt` fragment specifier can often fix these macros.
- [Fully de-stabilize the `#[bench]` attribute](https://redirect.github.com/rust-lang/rust/pull/134273). Usage of `#[bench]` without `#![feature(custom_test_frameworks)]` already triggered a deny-by-default future-incompatibility lint since Rust 1.77, but will now become a hard error.
- [Fix borrow checking some always-true patterns.](https://redirect.github.com/rust-lang/rust/pull/139042)
  The borrow checker was overly permissive in some cases, allowing programs that shouldn't have compiled.
- [Update the minimum external LLVM to 19.](https://redirect.github.com/rust-lang/rust/pull/139275)
- [Make it a hard error to use a vector type with a non-Rust ABI without enabling the required target feature.](https://redirect.github.com/rust-lang/rust/pull/139309)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Every minute ( * * * * * ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/clap-rs/clap).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC42Mi4xIiwidXBkYXRlZEluVmVyIjoiNDAuNjIuMSIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6W119-->


---
