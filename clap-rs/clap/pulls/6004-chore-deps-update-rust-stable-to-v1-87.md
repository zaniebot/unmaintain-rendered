---
number: 6004
title: "chore(deps): Update Rust Stable to v1.87"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-05-15T23:46:13Z
updated_at: 2025-05-16T13:17:46Z
url: https://github.com/clap-rs/clap/pull/6004
synced_at: 2026-01-10T01:28:26Z
---

# chore(deps): Update Rust Stable to v1.87

---

_Pull request opened by @renovate on 2025-05-15 23:46_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.86` -> `1.87` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.87`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1870-2025-05-15)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.86.0...1.87.0)

\==========================

<a id="1.87.0-Language"></a>

## Language

-   [Stabilize `asm_goto` feature](https://redirect.github.com/rust-lang/rust/pull/133870)
-   [Allow parsing open beginning ranges (`..EXPR`) after unary operators `!`, `-`, and `*`](https://redirect.github.com/rust-lang/rust/pull/134900).
-   [Don't require method impls for methods with `Self: Sized` bounds in `impl`s for unsized types](https://redirect.github.com/rust-lang/rust/pull/135480)
-   [Stabilize `feature(precise_capturing_in_traits)` allowing `use<...>` bounds on return position `impl Trait` in `trait`s](https://redirect.github.com/rust-lang/rust/pull/138128)

<a id="1.87.0-Compiler"></a>

## Compiler

-   [x86: make SSE2 required for i686 targets and use it to pass SIMD types](https://redirect.github.com/rust-lang/rust/pull/135408)

<a id="1.87.0-Platform-Support"></a>

## Platform Support

-   [Remove `i586-pc-windows-msvc` target](https://redirect.github.com/rust-lang/rust/pull/137957)

Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.

[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html

<a id="1.87.0-Libraries"></a>

## Libraries

-   [Stabilize the anonymous pipe API](https://redirect.github.com/rust-lang/rust/issues/127154)
-   [Add support for unbounded left/right shift operations](https://redirect.github.com/rust-lang/rust/issues/129375)
-   [Print pointer metadata in `Debug` impl of raw pointers](https://redirect.github.com/rust-lang/rust/pull/135080)
-   [`Vec::with_capacity` guarantees it allocates with the amount requested, even if `Vec::capacity` returns a different number.](https://redirect.github.com/rust-lang/rust/pull/135933)
-   Most `std::arch` intrinsics which don't take pointer arguments can now be called from safe code if the caller has the appropriate target features already enabled ([https://github.com/rust-lang/stdarch/pull/1714](https://redirect.github.com/rust-lang/stdarch/pull/1714), [https://github.com/rust-lang/stdarch/pull/1716](https://redirect.github.com/rust-lang/stdarch/pull/1716), [https://github.com/rust-lang/stdarch/pull/1717](https://redirect.github.com/rust-lang/stdarch/pull/1717))
-   [Undeprecate `env::home_dir`](https://redirect.github.com/rust-lang/rust/pull/137327)
-   [Denote `ControlFlow` as `#[must_use]`](https://redirect.github.com/rust-lang/rust/pull/137449)
-   [Macros such as `assert_eq!` and `vec!` now support `const {...}` expressions](https://redirect.github.com/rust-lang/rust/pull/138162)

<a id="1.87.0-Stabilized-APIs"></a>

## Stabilized APIs

-   [`Vec::extract_if`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.extract_if)
-   [`vec::ExtractIf`](https://doc.rust-lang.org/stable/std/vec/struct.ExtractIf.html)
-   [`LinkedList::extract_if`](https://doc.rust-lang.org/stable/std/collections/struct.LinkedList.html#method.extract_if)
-   [`linked_list::ExtractIf`](https://doc.rust-lang.org/stable/std/collections/linked_list/struct.ExtractIf.html)
-   [`<[T]>::split_off`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off)
-   [`<[T]>::split_off_mut`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off_mut)
-   [`<[T]>::split_off_first`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off_first)
-   [`<[T]>::split_off_first_mut`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off_first_mut)
-   [`<[T]>::split_off_last`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off_last)
-   [`<[T]>::split_off_last_mut`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.split_off_last_mut)
-   [`String::extend_from_within`](https://doc.rust-lang.org/stable/alloc/string/struct.String.html#method.extend_from_within)
-   [`os_str::Display`](https://doc.rust-lang.org/stable/std/ffi/os_str/struct.Display.html)
-   [`OsString::display`](https://doc.rust-lang.org/stable/std/ffi/struct.OsString.html#method.display)
-   [`OsStr::display`](https://doc.rust-lang.org/stable/std/ffi/struct.OsStr.html#method.display)
-   [`io::pipe`](https://doc.rust-lang.org/stable/std/io/fn.pipe.html)
-   [`io::PipeReader`](https://doc.rust-lang.org/stable/std/io/struct.PipeReader.html)
-   [`io::PipeWriter`](https://doc.rust-lang.org/stable/std/io/struct.PipeWriter.html)
-   [`impl From<PipeReader> for OwnedHandle`](https://doc.rust-lang.org/stable/std/os/windows/io/struct.OwnedHandle.html#impl-From%3CPipeReader%3E-for-OwnedHandle)
-   [`impl From<PipeWriter> for OwnedHandle`](https://doc.rust-lang.org/stable/std/os/windows/io/struct.OwnedHandle.html#impl-From%3CPipeWriter%3E-for-OwnedHandle)
-   [`impl From<PipeReader> for Stdio`](https://doc.rust-lang.org/stable/std/process/struct.Stdio.html)
-   [`impl From<PipeWriter> for Stdio`](https://doc.rust-lang.org/stable/std/process/struct.Stdio.html#impl-From%3CPipeWriter%3E-for-Stdio)
-   [`impl From<PipeReader> for OwnedFd`](https://doc.rust-lang.org/stable/std/os/fd/struct.OwnedFd.html#impl-From%3CPipeReader%3E-for-OwnedFd)
-   [`impl From<PipeWriter> for OwnedFd`](https://doc.rust-lang.org/stable/std/os/fd/struct.OwnedFd.html#impl-From%3CPipeWriter%3E-for-OwnedFd)
-   [`Box<MaybeUninit<T>>::write`](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html#method.write)
-   [`impl TryFrom<Vec<u8>> for String`](https://doc.rust-lang.org/stable/std/string/struct.String.html#impl-TryFrom%3CVec%3Cu8%3E%3E-for-String)
-   [`<*const T>::offset_from_unsigned`](https://doc.rust-lang.org/stable/std/primitive.pointer.html#method.offset_from_unsigned)
-   [`<*const T>::byte_offset_from_unsigned`](https://doc.rust-lang.org/stable/std/primitive.pointer.html#method.byte_offset_from_unsigned)
-   [`<*mut T>::offset_from_unsigned`](https://doc.rust-lang.org/stable/std/primitive.pointer.html#method.offset_from_unsigned-1)
-   [`<*mut T>::byte_offset_from_unsigned`](https://doc.rust-lang.org/stable/std/primitive.pointer.html#method.byte_offset_from_unsigned-1)
-   [`NonNull::offset_from_unsigned`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.offset_from_unsigned)
-   [`NonNull::byte_offset_from_unsigned`](https://doc.rust-lang.org/stable/std/ptr/struct.NonNull.html#method.byte_offset_from_unsigned)
-   [`<uN>::cast_signed`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.cast_signed)
-   [`NonZero::<uN>::cast_signed`](https://doc.rust-lang.org/stable/std/num/struct.NonZero.html#method.cast_signed-5).
-   [`<iN>::cast_unsigned`](https://doc.rust-lang.org/stable/std/primitive.isize.html#method.cast_unsigned).
-   [`NonZero::<iN>::cast_unsigned`](https://doc.rust-lang.org/stable/std/num/struct.NonZero.html#method.cast_unsigned-5).
-   [`<uN>::is_multiple_of`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.is_multiple_of)
-   [`<uN>::unbounded_shl`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.unbounded_shl)
-   [`<uN>::unbounded_shr`](https://doc.rust-lang.org/stable/std/primitive.usize.html#method.unbounded_shr)
-   [`<iN>::unbounded_shl`](https://doc.rust-lang.org/stable/std/primitive.isize.html#method.unbounded_shl)
-   [`<iN>::unbounded_shr`](https://doc.rust-lang.org/stable/std/primitive.isize.html#method.unbounded_shr)
-   [`<iN>::midpoint`](https://doc.rust-lang.org/stable/std/primitive.isize.html#method.midpoint)
-   [`<str>::from_utf8`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.from_utf8)
-   [`<str>::from_utf8_mut`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.from_utf8\_mut)
-   [`<str>::from_utf8_unchecked`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.from_utf8\_unchecked)
-   [`<str>::from_utf8_unchecked_mut`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.from_utf8\_unchecked_mut)

These previously stable APIs are now stable in const contexts:

-   [`core::str::from_utf8_mut`](https://doc.rust-lang.org/stable/std/str/fn.from_utf8\_mut.html)
-   [`<[T]>::copy_from_slice`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.copy_from_slice)
-   [`SocketAddr::set_ip`](https://doc.rust-lang.org/stable/std/net/enum.SocketAddr.html#method.set_ip)
-   [`SocketAddr::set_port`](https://doc.rust-lang.org/stable/std/net/enum.SocketAddr.html#method.set_port),
-   [`SocketAddrV4::set_ip`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV4.html#method.set_ip)
-   [`SocketAddrV4::set_port`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV4.html#method.set_port),
-   [`SocketAddrV6::set_ip`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV6.html#method.set_ip)
-   [`SocketAddrV6::set_port`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV6.html#method.set_port)
-   [`SocketAddrV6::set_flowinfo`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV6.html#method.set_flowinfo)
-   [`SocketAddrV6::set_scope_id`](https://doc.rust-lang.org/stable/std/net/struct.SocketAddrV6.html#method.set_scope_id)
-   [`char::is_digit`](https://doc.rust-lang.org/stable/std/primitive.char.html#method.is_digit)
-   [`char::is_whitespace`](https://doc.rust-lang.org/stable/std/primitive.char.html#method.is_whitespace)
-   [`<[[T; N]]>::as_flattened`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.as_flattened)
-   [`<[[T; N]]>::as_flattened_mut`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.as_flattened_mut)
-   [`String::into_bytes`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.into_bytes)
-   [`String::as_str`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.as_str)
-   [`String::capacity`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.capacity)
-   [`String::as_bytes`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.as_bytes)
-   [`String::len`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.len)
-   [`String::is_empty`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.is_empty)
-   [`String::as_mut_str`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.as_mut_str)
-   [`String::as_mut_vec`](https://doc.rust-lang.org/stable/std/string/struct.String.html#method.as_mut_vec)
-   [`Vec::as_ptr`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.as_ptr)
-   [`Vec::as_slice`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.as_slice)
-   [`Vec::capacity`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.capacity)
-   [`Vec::len`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.len)
-   [`Vec::is_empty`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.is_empty)
-   [`Vec::as_mut_slice`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.as_mut_slice)
-   [`Vec::as_mut_ptr`](https://doc.rust-lang.org/stable/std/vec/struct.Vec.html#method.as_mut_ptr)

<a id="1.87.0-Cargo"></a>

## Cargo

-   [Add terminal integration via ANSI OSC 9;4 sequences](https://redirect.github.com/rust-lang/cargo/pull/14615/)
-   [chore: bump openssl to v3](https://redirect.github.com/rust-lang/cargo/pull/15232/)
-   [feat(package): add --exclude-lockfile flag](https://redirect.github.com/rust-lang/cargo/pull/15234/)

<a id="1.87.0-Compatibility-Notes"></a>

## Compatibility Notes

-   [Rust now raises an error for macro invocations inside the `#![crate_name]` attribute](https://redirect.github.com/rust-lang/rust/pull/127581)
-   [Unstable fields are now always considered to be inhabited](https://redirect.github.com/rust-lang/rust/pull/133889)
-   [Macro arguments of unary operators followed by open beginning ranges may now be matched differently](https://redirect.github.com/rust-lang/rust/pull/134900)
-   [Make `Debug` impl of raw pointers print metadata if present](https://redirect.github.com/rust-lang/rust/pull/135080)
-   [Warn against function pointers using unsupported ABI strings in dependencies](https://redirect.github.com/rust-lang/rust/pull/135767)
-   [Associated types on `dyn` types are no longer deduplicated](https://redirect.github.com/rust-lang/rust/pull/136458)
-   [Forbid attributes on `..` inside of struct patterns (`let Struct { #[attribute] .. }) =`](https://redirect.github.com/rust-lang/rust/pull/136490)
-   [Make `ptr_cast_add_auto_to_object` lint into hard error](https://redirect.github.com/rust-lang/rust/pull/136764)
-   Many `std::arch` intrinsics are now safe to call in some contexts, there may now be new `unused_unsafe` warnings in existing codebases.
-   [Limit `width` and `precision` formatting options to 16 bits on all targets](https://redirect.github.com/rust-lang/rust/pull/136932)
-   [Turn order dependent trait objects future incompat warning into a hard error](https://redirect.github.com/rust-lang/rust/pull/136968)
-   [Denote `ControlFlow` as `#[must_use]`](https://redirect.github.com/rust-lang/rust/pull/137449)
-   [Windows: The standard library no longer links `advapi32`, except on win7.](https://redirect.github.com/rust-lang/rust/pull/138233) Code such as C libraries that were relying on this assumption may need to explicitly link advapi32.
-   [Proc macros can no longer observe expanded `cfg(true)` attributes.](https://redirect.github.com/rust-lang/rust/pull/138844)
-   [Start changing the internal representation of pasted tokens](https://redirect.github.com/rust-lang/rust/pull/124141). Certain invalid declarative macros that were previously accepted in obscure circumstances are now correctly rejected by the compiler. Use of a `tt` fragment specifier can often fix these macros.
-   [Don't allow flattened format_args in const.](https://redirect.github.com/rust-lang/rust/pull/139624)

<a id="1.87.0-Internal-Changes"></a>

## Internal Changes

These changes do not affect any public interfaces of Rust, but they represent
significant improvements to the performance or internals of rustc and related
tools.

-   [Update to LLVM 20](https://redirect.github.com/rust-lang/rust/pull/135763)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Every minute ( * * * * * ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Enabled.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/clap-rs/clap).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MC4xMS4xOCIsInVwZGF0ZWRJblZlciI6IjQwLjExLjE4IiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
