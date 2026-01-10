---
number: 6093
title: "chore(deps): Update Rust Stable to v1.89"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-08-07T13:58:33Z
updated_at: 2025-08-07T14:48:48Z
url: https://github.com/clap-rs/clap/pull/6093
synced_at: 2026-01-10T01:28:27Z
---

# chore(deps): Update Rust Stable to v1.89

---

_Pull request opened by @renovate on 2025-08-07 13:58_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.88` -> `1.89` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.89`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1890-2025-08-07)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.88.0...1.89.0)

\==========================

<a id="1.89.0-Language"></a>

## Language

- [Stabilize explicitly inferred const arguments (`feature(generic_arg_infer)`)](https://redirect.github.com/rust-lang/rust/pull/141610)
- [Add a warn-by-default `mismatched_lifetime_syntaxes` lint.](https://redirect.github.com/rust-lang/rust/pull/138677)
  This lint detects when the same lifetime is referred to by different syntax categories between function arguments and return values, which can be confusing to read, especially in unsafe code.
  This lint supersedes the warn-by-default `elided_named_lifetimes` lint.
- [Expand `unpredictable_function_pointer_comparisons` to also lint on function pointer comparisons in external macros](https://redirect.github.com/rust-lang/rust/pull/134536)
- [Make the `dangerous_implicit_autorefs` lint deny-by-default](https://redirect.github.com/rust-lang/rust/pull/141661)
- [Stabilize the avx512 target features](https://redirect.github.com/rust-lang/rust/pull/138940)
- [Stabilize `kl` and `widekl` target features for x86](https://redirect.github.com/rust-lang/rust/pull/140766)
- [Stabilize `sha512`, `sm3` and `sm4` target features for x86](https://redirect.github.com/rust-lang/rust/pull/140767)
- [Stabilize LoongArch target features `f`, `d`, `frecipe`, `lasx`, `lbt`, `lsx`, and `lvz`](https://redirect.github.com/rust-lang/rust/pull/135015)
- [Remove `i128` and `u128` from `improper_ctypes_definitions`](https://redirect.github.com/rust-lang/rust/pull/137306)
- [Stabilize `repr128` (`#[repr(u128)]`, `#[repr(i128)]`)](https://redirect.github.com/rust-lang/rust/pull/138285)
- [Allow `#![doc(test(attr(..)))]` everywhere](https://redirect.github.com/rust-lang/rust/pull/140560)
- [Extend temporary lifetime extension to also go through tuple struct and tuple variant constructors](https://redirect.github.com/rust-lang/rust/pull/140593)
- [`extern "C"` functions on the `wasm32-unknown-unknown` target now have a standards compliant ABI](https://blog.rust-lang.org/2025/04/04/c-abi-changes-for-wasm32-unknown-unknown/)

<a id="1.89.0-Compiler"></a>

## Compiler

- [Default to non-leaf frame pointers on aarch64-linux](https://redirect.github.com/rust-lang/rust/pull/140832)
- [Enable non-leaf frame pointers for Arm64EC Windows](https://redirect.github.com/rust-lang/rust/pull/140862)
- [Set Apple frame pointers by architecture](https://redirect.github.com/rust-lang/rust/pull/141797)

<a id="1.89.0-Platform-Support"></a>

## Platform Support

- [Add new Tier-3 targets `loongarch32-unknown-none` and `loongarch32-unknown-none-softfloat`](https://redirect.github.com/rust-lang/rust/pull/142053)
- [`x86_64-apple-darwin` is in the process of being demoted to Tier 2 with host tools](https://redirect.github.com/rust-lang/rfcs/pull/3841)

Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.

[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html

<a id="1.89.0-Libraries"></a>

## Libraries

- [Specify the base path for `file!`](https://redirect.github.com/rust-lang/rust/pull/134442)
- [Allow storing `format_args!()` in a variable](https://redirect.github.com/rust-lang/rust/pull/140748)
- [Add `#[must_use]` to `[T; N]::map`](https://redirect.github.com/rust-lang/rust/pull/140957)
- [Implement `DerefMut` for `Lazy{Cell,Lock}`](https://redirect.github.com/rust-lang/rust/pull/129334)
- [Implement `Default` for `array::IntoIter`](https://redirect.github.com/rust-lang/rust/pull/141574)
- [Implement `Clone` for `slice::ChunkBy`](https://redirect.github.com/rust-lang/rust/pull/138016)
- [Implement `io::Seek` for `io::Take`](https://redirect.github.com/rust-lang/rust/pull/138023)

<a id="1.89.0-Stabilized-APIs"></a>

## Stabilized APIs

- [`NonZero<char>`](https://doc.rust-lang.org/stable/std/num/struct.NonZero.html)
- Many intrinsics for x86, not enumerated here
  - [AVX512 intrinsics](https://redirect.github.com/rust-lang/rust/issues/111137)
  - [`SHA512`, `SM3` and `SM4` intrinsics](https://redirect.github.com/rust-lang/rust/issues/126624)
- [`File::lock`](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.lock)
- [`File::lock_shared`](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.lock_shared)
- [`File::try_lock`](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.try_lock)
- [`File::try_lock_shared`](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.try_lock_shared)
- [`File::unlock`](https://doc.rust-lang.org/stable/std/fs/struct.File.html#method.unlock)
- [`NonNull::from_ref`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.from_ref)
- [`NonNull::from_mut`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.from_mut)
- [`NonNull::without_provenance`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.without_provenance)
- [`NonNull::with_exposed_provenance`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.with_exposed_provenance)
- [`NonNull::expose_provenance`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.expose_provenance)
- [`OsString::leak`](https://doc.rust-lang.org/stable/std/ffi/struct.OsString.html#method.leak)
- [`PathBuf::leak`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#method.leak)
- [`Result::flatten`](https://doc.rust-lang.org/stable/std/result/enum.Result.html#method.flatten)
- [`std::os::linux::net::TcpStreamExt::quickack`](https://doc.rust-lang.org/stable/std/os/linux/net/trait.TcpStreamExt.html#tymethod.quickack)
- [`std::os::linux::net::TcpStreamExt::set_quickack`](https://doc.rust-lang.org/stable/std/os/linux/net/trait.TcpStreamExt.html#tymethod.set_quickack)

These previously stable APIs are now stable in const contexts:

- [`<[T; N]>::as_mut_slice`](https://doc.rust-lang.org/stable/std/primitive.array.html#method.as_mut_slice)
- [`<[u8]>::eq_ignore_ascii_case`](https://doc.rust-lang.org/stable/std/primitive.slice.html#impl-%5Bu8%5D/method.eq_ignore_ascii_case)
- [`str::eq_ignore_ascii_case`](https://doc.rust-lang.org/stable/std/primitive.str.html#impl-str/method.eq_ignore_ascii_case)

<a id="1.89.0-Cargo"></a>

## Cargo

- [`cargo fix` and `cargo clippy --fix` now default to the same Cargo target selection as other build commands.](https://redirect.github.com/rust-lang/cargo/pull/15192/) Previously it would apply to all targets (like binaries, examples, tests, etc.). The `--edition` flag still applies to all targets.
- [Stabilize doctest-xcompile.](https://redirect.github.com/rust-lang/cargo/pull/15462/) Doctests are now tested when cross-compiling. Just like other tests, it will use the [`runner` setting](https://doc.rust-lang.org/cargo/reference/config.html#targettriplerunner) to run the tests. If you need to disable tests for a target, you can use the [ignore doctest attribute](https://doc.rust-lang.org/rustdoc/write-documentation/documentation-tests.html#ignoring-targets) to specify the targets to ignore.

<a id="1.89.0-Rustdoc"></a>

## Rustdoc

- [On mobile, make the sidebar full width and linewrap](https://redirect.github.com/rust-lang/rust/pull/139831). This makes long section and item names much easier to deal with on mobile.

<a id="1.89.0-Compatibility-Notes"></a>

## Compatibility Notes

- [Make `missing_fragment_specifier` an unconditional error](https://redirect.github.com/rust-lang/rust/pull/128425)
- [Enabling the `neon` target feature on `aarch64-unknown-none-softfloat` causes a warning](https://redirect.github.com/rust-lang/rust/pull/135160) because mixing code with and without that target feature is not properly supported by LLVM
- [Sized Hierarchy: Part I](https://redirect.github.com/rust-lang/rust/pull/137944)
  - Introduces a small breaking change affecting `?Sized` bounds on impls on recursive types which contain associated type projections. It is not expected to affect any existing published crates. Can be fixed by refactoring the involved types or opting into the `sized_hierarchy` unstable feature. See the [FCP report](https://redirect.github.com/rust-lang/rust/pull/137944#issuecomment-2912207485) for a code example.
- The warn-by-default `elided_named_lifetimes` lint is [superseded by the warn-by-default `mismatched_lifetime_syntaxes` lint.](https://redirect.github.com/rust-lang/rust/pull/138677)
- [Error on recursive opaque types earlier in the type checker](https://redirect.github.com/rust-lang/rust/pull/139419)
- [Type inference side effects from requiring element types of array repeat expressions are `Copy` are now only available at the end of type checking](https://redirect.github.com/rust-lang/rust/pull/139635)
- [The deprecated accidentally-stable `std::intrinsics::{copy,copy_nonoverlapping,write_bytes}` are now proper intrinsics](https://redirect.github.com/rust-lang/rust/pull/139916). There are no debug assertions guarding against UB, and they cannot be coerced to function pointers.
- [Remove long-deprecated `std::intrinsics::drop_in_place`](https://redirect.github.com/rust-lang/rust/pull/140151)
- [Make well-formedness predicates no longer coinductive](https://redirect.github.com/rust-lang/rust/pull/140208)
- [Remove hack when checking impl method compatibility](https://redirect.github.com/rust-lang/rust/pull/140557)
- [Remove unnecessary type inference due to built-in trait object impls](https://redirect.github.com/rust-lang/rust/pull/141352)
- [Lint against "stdcall", "fastcall", and "cdecl" on non-x86-32 targets](https://redirect.github.com/rust-lang/rust/pull/141435)
- [Future incompatibility warnings relating to the never type (`!`) are now reported in dependencies](https://redirect.github.com/rust-lang/rust/pull/141937)
- [Ensure `std::ptr::copy_*` intrinsics also perform the static self-init checks](https://redirect.github.com/rust-lang/rust/pull/142575)
- [`extern "C"` functions on the `wasm32-unknown-unknown` target now have a standards compliant ABI](https://blog.rust-lang.org/2025/04/04/c-abi-changes-for-wasm32-unknown-unknown/)

<a id="1.89.0-Internal-Changes"></a>

## Internal Changes

These changes do not affect any public interfaces of Rust, but they represent
significant improvements to the performance or internals of rustc and related
tools.

- [Correctly un-remap compiler sources paths with the `rustc-dev` component](https://redirect.github.com/rust-lang/rust/pull/142377)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS41MS4xIiwidXBkYXRlZEluVmVyIjoiNDEuNTEuMSIsInRhcmdldEJyYW5jaCI6Im1hc3RlciIsImxhYmVscyI6W119-->


---
