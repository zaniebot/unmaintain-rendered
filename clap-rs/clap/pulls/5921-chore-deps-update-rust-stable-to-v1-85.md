---
number: 5921
title: "chore(deps): Update Rust Stable to v1.85"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-02-20T19:41:04Z
updated_at: 2025-02-20T19:48:51Z
url: https://github.com/clap-rs/clap/pull/5921
synced_at: 2026-01-10T01:28:25Z
---

# chore(deps): Update Rust Stable to v1.85

---

_Pull request opened by @renovate on 2025-02-20 19:41_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.84` -> `1.85` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.85`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1850-2025-02-20)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.84.0...1.85.0)

\==========================

<a id="1.85.0-Language"></a>

## Language

-   [The 2024 Edition is now stable.](https://redirect.github.com/rust-lang/rust/pull/133349)
    See [the edition guide](https://doc.rust-lang.org/nightly/edition-guide/rust-2024/index.html) for more details.
-   [Stabilize async closures](https://redirect.github.com/rust-lang/rust/pull/132706)
    See [RFC 3668](https://rust-lang.github.io/rfcs/3668-async-closures.html) for more details.
-   [Stabilize `#[diagnostic::do_not_recommend]`](https://redirect.github.com/rust-lang/rust/pull/132056)
-   [Add `unpredictable_function_pointer_comparisons` lint to warn against function pointer comparisons](https://redirect.github.com/rust-lang/rust/pull/118833)
-   [Lint on combining `#[no_mangle]` and `#[export_name]` attributes.](https://redirect.github.com/rust-lang/rust/pull/131558)

<a id="1.85.0-Compiler"></a>

## Compiler

-   [The unstable flag `-Zpolymorphize` has been removed](https://redirect.github.com/rust-lang/rust/pull/133883), see [https://github.com/rust-lang/compiler-team/issues/810](https://redirect.github.com/rust-lang/compiler-team/issues/810) for some background.

<a id="1.85.0-Platform-Support"></a>

## Platform Support

-   [Promote `powerpc64le-unknown-linux-musl` to tier 2 with host tools](https://redirect.github.com/rust-lang/rust/pull/133801)

Refer to Rust's \[platform support page]\[platform-support-doc]
for more information on Rust's tiered platform support.

<a id="1.85.0-Libraries"></a>

## Libraries

-   [Panics in the standard library now have a leading `library/` in their path](https://redirect.github.com/rust-lang/rust/pull/132390)
-   [`std::env::home_dir()` on Windows now ignores the non-standard `$HOME` environment variable](https://redirect.github.com/rust-lang/rust/pull/132515)

    It will be un-deprecated in a subsequent release.
-   [Add `AsyncFn*` to the prelude in all editions.](https://redirect.github.com/rust-lang/rust/pull/132611)

<a id="1.85.0-Stabilized-APIs"></a>

## Stabilized APIs

-   [`BuildHasherDefault::new`](https://doc.rust-lang.org/stable/std/hash/struct.BuildHasherDefault.html#method.new)
-   [`ptr::fn_addr_eq`](https://doc.rust-lang.org/std/ptr/fn.fn_addr_eq.html)
-   [`io::ErrorKind::QuotaExceeded`](https://doc.rust-lang.org/stable/std/io/enum.ErrorKind.html#variant.QuotaExceeded)
-   [`io::ErrorKind::CrossesDevices`](https://doc.rust-lang.org/stable/std/io/enum.ErrorKind.html#variant.CrossesDevices)
-   [`{float}::midpoint`](https://doc.rust-lang.org/core/primitive.f32.html#method.midpoint)
-   [Unsigned `{integer}::midpoint`](https://doc.rust-lang.org/std/primitive.u64.html#method.midpoint)
-   [`NonZeroU*::midpoint`](https://doc.rust-lang.org/std/num/type.NonZeroU32.html#method.midpoint)
-   [impl `std::iter::Extend` for tuples with arity 1 through 12](https://doc.rust-lang.org/stable/std/iter/trait.Extend.html#impl-Extend%3C\(A,\)%3E-for-\(EA,\))
-   [`FromIterator<(A, ...)>` for tuples with arity 1 through 12](https://doc.rust-lang.org/stable/std/iter/trait.FromIterator.html#impl-FromIterator%3C\(EA,\)%3E-for-\(A,\))
-   [`std::task::Waker::noop`](https://doc.rust-lang.org/stable/std/task/struct.Waker.html#method.noop)

These APIs are now stable in const contexts:

-   [`mem::size_of_val`](https://doc.rust-lang.org/stable/std/mem/fn.size_of_val.html)
-   [`mem::align_of_val`](https://doc.rust-lang.org/stable/std/mem/fn.align_of_val.html)
-   [`Layout::for_value`](https://doc.rust-lang.org/stable/std/alloc/struct.Layout.html#method.for_value)
-   [`Layout::align_to`](https://doc.rust-lang.org/stable/std/alloc/struct.Layout.html#method.align_to)
-   [`Layout::pad_to_align`](https://doc.rust-lang.org/stable/std/alloc/struct.Layout.html#method.pad_to_align)
-   [`Layout::extend`](https://doc.rust-lang.org/stable/std/alloc/struct.Layout.html#method.extend)
-   [`Layout::array`](https://doc.rust-lang.org/stable/std/alloc/struct.Layout.html#method.array)
-   [`std::mem::swap`](https://doc.rust-lang.org/stable/std/mem/fn.swap.html)
-   [`std::ptr::swap`](https://doc.rust-lang.org/stable/std/ptr/fn.swap.html)
-   [`NonNull::new`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.new)
-   [`HashMap::with_hasher`](https://doc.rust-lang.org/stable/std/collections/struct.HashMap.html#method.with_hasher)
-   [`HashSet::with_hasher`](https://doc.rust-lang.org/stable/std/collections/struct.HashSet.html#method.with_hasher)
-   [`BuildHasherDefault::new`](https://doc.rust-lang.org/stable/std/hash/struct.BuildHasherDefault.html#method.new)
-   [`<float>::recip`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.recip)
-   [`<float>::to_degrees`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.to_degrees)
-   [`<float>::to_radians`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.to_radians)
-   [`<float>::max`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.max)
-   [`<float>::min`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.min)
-   [`<float>::clamp`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.clamp)
-   [`<float>::abs`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.abs)
-   [`<float>::signum`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.signum)
-   [`<float>::copysign`](https://doc.rust-lang.org/stable/std/primitive.f32.html#method.copysign)
-   [`MaybeUninit::write`](https://doc.rust-lang.org/stable/std/mem/union.MaybeUninit.html#method.write)

<a id="1.85.0-Cargo"></a>

## Cargo

-   [Add future-incompatibility warning against keywords in cfgs and add raw-idents](https://redirect.github.com/rust-lang/cargo/pull/14671/)
-   [Stabilize higher precedence trailing flags](https://redirect.github.com/rust-lang/cargo/pull/14900/)
-   [Pass `CARGO_CFG_FEATURE` to build scripts](https://redirect.github.com/rust-lang/cargo/pull/14902/)

<a id="1.85.0-Rustdoc"></a>

## Rustdoc

-   [Doc comment on impl blocks shows the first line, even when the impl block is collapsed](https://redirect.github.com/rust-lang/rust/pull/132155)

<a id="1.85.0-Compatibility-Notes"></a>

## Compatibility Notes

-   [`rustc` no longer treats the `test` cfg as a well known check-cfg](https://redirect.github.com/rust-lang/rust/pull/131729), instead it is up to the build systems and users of `--check-cfg`\[^check-cfg] to set it as a well known cfg using `--check-cfg=cfg(test)`.

    This is done to enable build systems like Cargo to set it conditionally, as not all source files are suitable for unit tests.
    [Cargo (for now) unconditionally sets the `test` cfg as a well known cfg](https://redirect.github.com/rust-lang/cargo/pull/14963).
    \[^check-cfg]: https://doc.rust-lang.org/nightly/rustc/check-cfg.html
-   [Disable potentially incorrect type inference if there are trivial and non-trivial where-clauses](https://redirect.github.com/rust-lang/rust/pull/132325)
-   `std::env::home_dir()` has been deprecated for years, because it can give surprising results in some Windows configurations if the `HOME` environment variable is set (which is not the normal configuration on Windows). We had previously avoided changing its behavior, out of concern for compatibility with code depending on this non-standard configuration. Given how long this function has been deprecated, we're now fixing its behavior as a bugfix. A subsequent release will remove the deprecation for this function.
-   [Make `core::ffi::c_char` signedness more closely match that of the platform-default `char`](https://redirect.github.com/rust-lang/rust/pull/132975)

    This changed `c_char` from an `i8` to `u8` or vice versa on many Tier 2 and 3
    targets (mostly Arm and RISC-V embedded targets). The new definition may
    result in compilation failures but fixes compatibility issues with C.

    The `libc` crate matches this change as of its 0.2.169 release.
-   [When compiling a nested `macro_rules` macro from an external crate, the content of the inner `macro_rules` is now built with the edition of the external crate, not the local crate.](https://redirect.github.com/rust-lang/rust/pull/133274)
-   [Increase `sparcv9-sun-solaris` and `x86_64-pc-solaris` Solaris baseline to 11.4.](https://redirect.github.com/rust-lang/rust/pull/133293)
-   [Show `abi_unsupported_vector_types` lint in future breakage reports](https://redirect.github.com/rust-lang/rust/pull/133374)
-   [Error if multiple super-trait instantiations of `dyn Trait` need associated types to be specified but only one is provided](https://redirect.github.com/rust-lang/rust/pull/133392)
-   [Change `powerpc64-ibm-aix` default `codemodel` to large](https://redirect.github.com/rust-lang/rust/pull/133811)

<a id="1.85.0-Internal-Changes"></a>

## Internal Changes

These changes do not affect any public interfaces of Rust, but they represent
significant improvements to the performance or internals of rustc and related
tools.

-   [Build `x86_64-unknown-linux-gnu` with LTO for C/C++ code (e.g., `jemalloc`)](https://redirect.github.com/rust-lang/rust/pull/134690)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "* * * * *" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR is behind base branch, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/clap-rs/clap).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4xNzYuMiIsInVwZGF0ZWRJblZlciI6IjM5LjE3Ni4yIiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
