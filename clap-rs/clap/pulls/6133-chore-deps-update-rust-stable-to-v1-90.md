---
number: 6133
title: "chore(deps): Update Rust Stable to v1.90"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-09-18T22:35:41Z
updated_at: 2025-09-18T22:43:28Z
url: https://github.com/clap-rs/clap/pull/6133
synced_at: 2026-01-10T01:28:28Z
---

# chore(deps): Update Rust Stable to v1.90

---

_Pull request opened by @renovate on 2025-09-18 22:35_

Coming soon: The Renovate bot (GitHub App) will be renamed to Mend. PRs from Renovate will soon appear from 'Mend'. Learn more [here](https://redirect.github.com/renovatebot/renovate/discussions/37842).

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.89` -> `1.90` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.90`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1900-2025-09-18)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.89.0...1.90.0)

\===========================

<a id="1.90-Language"></a>

## Language

- [Split up the `unknown_or_malformed_diagnostic_attributes` lint](https://redirect.github.com/rust-lang/rust/pull/140717). This lint has been split up into four finer-grained lints, with `unknown_or_malformed_diagnostic_attributes` now being the lint group that contains these lints:
  1. `unknown_diagnostic_attributes`: unknown to the current compiler
  2. `misplaced_diagnostic_attributes`: placed on the wrong item
  3. `malformed_diagnostic_attributes`: malformed attribute syntax or options
  4. `malformed_diagnostic_format_literals`: malformed format string literal
- [Allow constants whose final value has references to mutable/external memory, but reject such constants as patterns](https://redirect.github.com/rust-lang/rust/pull/140942)
- [Allow volatile access to non-Rust memory, including address 0](https://redirect.github.com/rust-lang/rust/pull/141260)

<a id="1.90-Compiler"></a>

## Compiler

- [Use `lld` by default on `x86_64-unknown-linux-gnu`](https://redirect.github.com/rust-lang/rust/pull/140525).
- [Tier 3 `musl` targets now link dynamically by default](https://redirect.github.com/rust-lang/rust/pull/144410). Affected targets:
  - `mips64-unknown-linux-muslabi64`
  - `powerpc64-unknown-linux-musl`
  - `powerpc-unknown-linux-musl`
  - `powerpc-unknown-linux-muslspe`
  - `riscv32gc-unknown-linux-musl`
  - `s390x-unknown-linux-musl`
  - `thumbv7neon-unknown-linux-musleabihf`

<a id="1.90-Platform-Support"></a>

## Platform Support

- [Demote `x86_64-apple-darwin` to Tier 2 with host tools](https://redirect.github.com/rust-lang/rust/pull/145252)

Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.

[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html

<a id="1.90-Libraries"></a>

## Libraries

- [Stabilize `u*::{checked,overflowing,saturating,wrapping}_sub_signed`](https://redirect.github.com/rust-lang/rust/issues/126043)
- [Allow comparisons between `CStr`, `CString`, and `Cow<CStr>`](https://redirect.github.com/rust-lang/rust/pull/137268)
- [Remove some unsized tuple impls since unsized tuples can't be constructed](https://redirect.github.com/rust-lang/rust/pull/138340)
- [Set `MSG_NOSIGNAL` for `UnixStream`](https://redirect.github.com/rust-lang/rust/pull/140005)
- [`proc_macro::Ident::new` now supports `$crate`.](https://redirect.github.com/rust-lang/rust/pull/141996)
- [Guarantee the pointer returned from `Thread::into_raw` has at least 8 bytes of alignment](https://redirect.github.com/rust-lang/rust/pull/143859)

<a id="1.90-Stabilized-APIs"></a>

## Stabilized APIs

- [`u{n}::checked_sub_signed`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.checked_sub_signed)
- [`u{n}::overflowing_sub_signed`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.overflowing_sub_signed)
- [`u{n}::saturating_sub_signed`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.saturating_sub_signed)
- [`u{n}::wrapping_sub_signed`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.wrapping_sub_signed)
- [`impl Copy for IntErrorKind`](https://doc.rust-lang.org/stable/std/num/enum.IntErrorKind.html#impl-Copy-for-IntErrorKind)
- [`impl Hash for IntErrorKind`](https://doc.rust-lang.org/stable/std/num/enum.IntErrorKind.html#impl-Hash-for-IntErrorKind)
- [`impl PartialEq<&CStr> for CStr`](https://doc.rust-lang.org/stable/std/ffi/struct.CStr.html#impl-PartialEq%3C%26CStr%3E-for-CStr)
- [`impl PartialEq<CString> for CStr`](https://doc.rust-lang.org/stable/std/ffi/struct.CStr.html#impl-PartialEq%3CCString%3E-for-CStr)
- [`impl PartialEq<Cow<CStr>> for CStr`](https://doc.rust-lang.org/stable/std/ffi/struct.CStr.html#impl-PartialEq%3CCow%3C'_,+CStr%3E%3E-for-CStr)
- [`impl PartialEq<&CStr> for CString`](https://doc.rust-lang.org/stable/std/ffi/struct.CString.html#impl-PartialEq%3C%26CStr%3E-for-CString)
- [`impl PartialEq<CStr> for CString`](https://doc.rust-lang.org/stable/std/ffi/struct.CString.html#impl-PartialEq%3CCStr%3E-for-CString)
- [`impl PartialEq<Cow<CStr>> for CString`](https://doc.rust-lang.org/stable/std/ffi/struct.CString.html#impl-PartialEq%3CCow%3C'_,+CStr%3E%3E-for-CString)
- [`impl PartialEq<&CStr> for Cow<CStr>`](https://doc.rust-lang.org/stable/std/borrow/enum.Cow.html#impl-PartialEq%3C%26CStr%3E-for-Cow%3C'_,+CStr%3E)
- [`impl PartialEq<CStr> for Cow<CStr>`](https://doc.rust-lang.org/stable/std/borrow/enum.Cow.html#impl-PartialEq%3CCStr%3E-for-Cow%3C'_,+CStr%3E)
- [`impl PartialEq<CString> for Cow<CStr>`](https://doc.rust-lang.org/stable/std/borrow/enum.Cow.html#impl-PartialEq%3CCString%3E-for-Cow%3C'_,+CStr%3E)

These previously stable APIs are now stable in const contexts:

- [`<[T]>::reverse`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.reverse)
- [`f32::floor`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.floor)
- [`f32::ceil`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.ceil)
- [`f32::trunc`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.trunc)
- [`f32::fract`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.fract)
- [`f32::round`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.round)
- [`f32::round_ties_even`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.round_ties_even)
- [`f64::floor`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.floor)
- [`f64::ceil`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.ceil)
- [`f64::trunc`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.trunc)
- [`f64::fract`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.fract)
- [`f64::round`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.round)
- [`f64::round_ties_even`](https://doc.rust-lang.org/stable/std/primitive.f64.html#method.round_ties_even)

<a id="1.90-Cargo"></a>

## Cargo

- [Add `http.proxy-cainfo` config for proxy certs](https://redirect.github.com/rust-lang/cargo/pull/15374/)
- [Use `gix` for `cargo package`](https://redirect.github.com/rust-lang/cargo/pull/15534/)
- [feat(publish): Stabilize multi-package publishing](https://redirect.github.com/rust-lang/cargo/pull/15636/)

<a id="1.90-Rustdoc"></a>

## Rustdoc

- [Add ways to collapse all impl blocks](https://redirect.github.com/rust-lang/rust/pull/141663). Previously the "Summary" button and "-" keyboard shortcut would never collapse `impl` blocks, now they do when shift is held
- [Display unsafe attributes with `unsafe()` wrappers](https://redirect.github.com/rust-lang/rust/pull/143662)

<a id="1.90-Compatibility-Notes"></a>

## Compatibility Notes

- [Use `lld` by default on `x86_64-unknown-linux-gnu`](https://redirect.github.com/rust-lang/rust/pull/140525).
  See also <https://blog.rust-lang.org/2025/09/01/rust-lld-on-1.90.0-stable/>.
- [Make `core::iter::Fuse`'s `Default` impl construct `I::default()` internally as promised in the docs instead of always being empty](https://redirect.github.com/rust-lang/rust/pull/140985)
- [Set `MSG_NOSIGNAL` for `UnixStream`](https://redirect.github.com/rust-lang/rust/pull/140005)
  This may change program behavior but results in the same behavior as other primitives (e.g., stdout, network sockets).
  Programs relying on signals to terminate them should update handling of sockets to handle errors on write by exiting.
- [On Unix `std::env::home_dir` will use the fallback if the `HOME` environment variable is empty](https://redirect.github.com/rust-lang/rust/pull/141840)
- We now [reject unsupported `extern "{abi}"`s consistently in all positions](https://redirect.github.com/rust-lang/rust/pull/142134). This primarily affects the use of implementing traits on an `extern "{abi}"` function pointer, like `extern "stdcall" fn()`, on a platform that doesn't support that, like aarch64-unknown-linux-gnu. Direct usage of these unsupported ABI strings by declaring or defining functions was already rejected, so this is only a change for consistency.
- [const-eval: error when initializing a static writes to that static](https://redirect.github.com/rust-lang/rust/pull/143084)
- [Check that the `proc_macro_derive` macro has correct arguments when applied to the crate root](https://redirect.github.com/rust-lang/rust/pull/143607)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS45Ny4xMCIsInVwZGF0ZWRJblZlciI6IjQxLjk3LjEwIiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
