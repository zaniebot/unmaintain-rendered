---
number: 6169
title: "chore(deps): Update Rust Stable to v1.91"
type: pull_request
state: merged
author: renovate
labels: []
assignees: []
merged: true
base: master
head: renovate/stable-1.x
created_at: 2025-10-31T00:46:30Z
updated_at: 2025-10-31T20:17:17Z
url: https://github.com/clap-rs/clap/pull/6169
synced_at: 2026-01-07T13:12:20-06:00
---

# chore(deps): Update Rust Stable to v1.91

---

_Pull request opened by @renovate on 2025-10-31 00:46_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [STABLE](https://redirect.github.com/rust-lang/rust) | minor | `1.90` -> `1.91` |

---

### Release Notes

<details>
<summary>rust-lang/rust (STABLE)</summary>

### [`v1.91`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1910-2025-10-30)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.90.0...1.91.0)

\==========================

<a id="1.91.0-Language"></a>

## Language

- [Lower pattern bindings in the order they're written and base drop order on primary bindings' order](https://redirect.github.com/rust-lang/rust/pull/143764)
- [Stabilize declaration of C-style variadic functions for `sysv64`, `win64`, `efiapi`, and `aapcs` ABIs](https://redirect.github.com/rust-lang/rust/pull/144066).
  This brings these ABIs in line with the C ABI: variadic functions can be declared in extern blocks but not defined.
- [Add `dangling_pointers_from_locals` lint to warn against dangling pointers from local variables](https://redirect.github.com/rust-lang/rust/pull/144322)
- [Upgrade `semicolon_in_expressions_from_macros` from warn to deny](https://redirect.github.com/rust-lang/rust/pull/144369)
- [Stabilize LoongArch32 inline assembly](https://redirect.github.com/rust-lang/rust/pull/144402)
- [Add warn-by-default `integer_to_ptr_transmutes` lint against integer-to-pointer transmutes](https://redirect.github.com/rust-lang/rust/pull/144531)
- [Stabilize `sse4a` and `tbm` target features](https://redirect.github.com/rust-lang/rust/pull/144542)
- [Add `target_env = "macabi"` and `target_env = "sim"` cfgs](https://redirect.github.com/rust-lang/rust/pull/139451) as replacements for the `target_abi` cfgs with the same values.

<a id="1.91.0-Compiler"></a>

## Compiler

- [Don't warn on never-to-any `as` casts as unreachable](https://redirect.github.com/rust-lang/rust/pull/144804)

<a id="1.91.0-Platform-Support"></a>

## Platform Support

- [Promote `aarch64-pc-windows-gnullvm` and `x86_64-pc-windows-gnullvm` to Tier 2 with host tools.](https://redirect.github.com/rust-lang/rust/pull/143031)
  Note: llvm-tools and MSI installers are missing but will be added in future releases.
- [Promote `aarch64-pc-windows-msvc` to Tier 1](https://redirect.github.com/rust-lang/rust/pull/145682)

Refer to Rust's [platform support page][platform-support-doc]
for more information on Rust's tiered platform support.

[platform-support-doc]: https://doc.rust-lang.org/rustc/platform-support.html

<a id="1.91.0-Libraries"></a>

## Libraries

- [Print thread ID in panic message](https://redirect.github.com/rust-lang/rust/pull/115746)
- [Fix overly restrictive lifetime in `core::panic::Location::file` return type](https://redirect.github.com/rust-lang/rust/pull/132087)
- [Guarantee parameter order for `_by()` variants of `min` / `max`/ `minmax` in `std::cmp`](https://redirect.github.com/rust-lang/rust/pull/139357)
- [Document assumptions about `Clone` and `Eq` traits](https://redirect.github.com/rust-lang/rust/pull/144330/)
- [`std::thread`: Return error if setting thread stack size fails](https://redirect.github.com/rust-lang/rust/pull/144210)
  This used to panic within the standard library.

<a id="1.91.0-Stabilized-APIs"></a>

## Stabilized APIs

- [`Path::file_prefix`](https://doc.rust-lang.org/stable/std/path/struct.Path.html#method.file_prefix)
- [`AtomicPtr::fetch_ptr_add`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_ptr_add)
- [`AtomicPtr::fetch_ptr_sub`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_ptr_sub)
- [`AtomicPtr::fetch_byte_add`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_byte_add)
- [`AtomicPtr::fetch_byte_sub`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_byte_sub)
- [`AtomicPtr::fetch_or`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_or)
- [`AtomicPtr::fetch_and`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_and)
- [`AtomicPtr::fetch_xor`](https://doc.rust-lang.org/stable/std/sync/atomic/struct.AtomicPtr.html#method.fetch_xor)
- [`{integer}::strict_add`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_add)
- [`{integer}::strict_sub`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_sub)
- [`{integer}::strict_mul`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_mul)
- [`{integer}::strict_div`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_div)
- [`{integer}::strict_div_euclid`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_div_euclid)
- [`{integer}::strict_rem`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_rem)
- [`{integer}::strict_rem_euclid`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_rem_euclid)
- [`{integer}::strict_neg`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_neg)
- [`{integer}::strict_shl`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_shl)
- [`{integer}::strict_shr`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_shr)
- [`{integer}::strict_pow`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_pow)
- [`i{N}::strict_add_unsigned`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_add_unsigned)
- [`i{N}::strict_sub_unsigned`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_sub_unsigned)
- [`i{N}::strict_abs`](https://doc.rust-lang.org/stable/std/primitive.i32.html#method.strict_abs)
- [`u{N}::strict_add_signed`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_add_signed)
- [`u{N}::strict_sub_signed`](https://doc.rust-lang.org/stable/std/primitive.u32.html#method.strict_sub_signed)
- [`PanicHookInfo::payload_as_str`](https://doc.rust-lang.org/stable/std/panic/struct.PanicHookInfo.html#method.payload_as_str)
- [`core::iter::chain`](https://doc.rust-lang.org/stable/core/iter/fn.chain.html)
- [`u{N}::checked_signed_diff`](https://doc.rust-lang.org/stable/std/primitive.u16.html#method.checked_signed_diff)
- [`core::array::repeat`](https://doc.rust-lang.org/stable/core/array/fn.repeat.html)
- [`PathBuf::add_extension`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#method.add_extension)
- [`PathBuf::with_added_extension`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#method.with_added_extension)
- [`Duration::from_mins`](https://doc.rust-lang.org/stable/std/time/struct.Duration.html#method.from_mins)
- [`Duration::from_hours`](https://doc.rust-lang.org/stable/std/time/struct.Duration.html#method.from_hours)
- [`impl PartialEq<str> for PathBuf`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#impl-PartialEq%3Cstr%3E-for-PathBuf)
- [`impl PartialEq<String> for PathBuf`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#impl-PartialEq%3CString%3E-for-PathBuf)
- [`impl PartialEq<str> for Path`](https://doc.rust-lang.org/stable/std/path/struct.Path.html#impl-PartialEq%3Cstr%3E-for-Path)
- [`impl PartialEq<String> for Path`](https://doc.rust-lang.org/stable/std/path/struct.Path.html#impl-PartialEq%3CString%3E-for-Path)
- [`impl PartialEq<PathBuf> for String`](https://doc.rust-lang.org/stable/std/string/struct.String.html#impl-PartialEq%3CPathBuf%3E-for-String)
- [`impl PartialEq<Path> for String`](https://doc.rust-lang.org/stable/std/string/struct.String.html#impl-PartialEq%3CPath%3E-for-String)
- [`impl PartialEq<PathBuf> for str`](https://doc.rust-lang.org/stable/std/primitive.str.html#impl-PartialEq%3CPathBuf%3E-for-str)
- [`impl PartialEq<Path> for str`](https://doc.rust-lang.org/stable/std/primitive.str.html#impl-PartialEq%3CPath%3E-for-str)
- [`Ipv4Addr::from_octets`](https://doc.rust-lang.org/stable/std/net/struct.Ipv4Addr.html#method.from_octets)
- [`Ipv6Addr::from_octets`](https://doc.rust-lang.org/stable/std/net/struct.Ipv6Addr.html#method.from_octets)
- [`Ipv6Addr::from_segments`](https://doc.rust-lang.org/stable/std/net/struct.Ipv6Addr.html#method.from_segments)
- [`impl<T> Default for Pin<Box<T>> where Box<T>: Default, T: ?Sized`](https://doc.rust-lang.org/stable/std/default/trait.Default.html#impl-Default-for-Pin%3CBox%3CT%3E%3E)
- [`impl<T> Default for Pin<Rc<T>> where Rc<T>: Default, T: ?Sized`](https://doc.rust-lang.org/stable/std/default/trait.Default.html#impl-Default-for-Pin%3CRc%3CT%3E%3E)
- [`impl<T> Default for Pin<Arc<T>> where Arc<T>: Default, T: ?Sized`](https://doc.rust-lang.org/stable/std/default/trait.Default.html#impl-Default-for-Pin%3CArc%3CT%3E%3E)
- [`Cell::as_array_of_cells`](https://doc.rust-lang.org/stable/std/cell/struct.Cell.html#method.as_array_of_cells)
- [`u{N}::carrying_add`](https://doc.rust-lang.org/stable/std/primitive.u64.html#method.carrying_add)
- [`u{N}::borrowing_sub`](https://doc.rust-lang.org/stable/std/primitive.u64.html#method.borrowing_sub)
- [`u{N}::carrying_mul`](https://doc.rust-lang.org/stable/std/primitive.u64.html#method.carrying_mul)
- [`u{N}::carrying_mul_add`](https://doc.rust-lang.org/stable/std/primitive.u64.html#method.carrying_mul_add)
- [`BTreeMap::extract_if`](https://doc.rust-lang.org/stable/std/collections/struct.BTreeMap.html#method.extract_if)
- [`BTreeSet::extract_if`](https://doc.rust-lang.org/stable/std/collections/struct.BTreeSet.html#method.extract_if)
- [`impl Debug for windows::ffi::EncodeWide<'_>`](https://doc.rust-lang.org/stable/std/os/windows/ffi/struct.EncodeWide.html#impl-Debug-for-EncodeWide%3C'_%3E)
- [`str::ceil_char_boundary`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.ceil_char_boundary)
- [`str::floor_char_boundary`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.floor_char_boundary)
- [`impl Sum for Saturating<u{N}>`](https://doc.rust-lang.org/stable/std/num/struct.Saturating.html#impl-Sum-for-Saturating%3Cu32%3E)
- [`impl Sum<&Self> for Saturating<u{N}>`](https://doc.rust-lang.org/stable/std/num/struct.Saturating.html#impl-Sum%3C%26Saturating%3Cu32%3E%3E-for-Saturating%3Cu32%3E)
- [`impl Product for Saturating<u{N}>`](https://doc.rust-lang.org/stable/std/num/struct.Saturating.html#impl-Product-for-Saturating%3Cu32%3E)
- [`impl Product<&Self> for Saturating<u{N}>`](https://doc.rust-lang.org/stable/std/num/struct.Saturating.html#impl-Product%3C%26Saturating%3Cu32%3E%3E-for-Saturating%3Cu32%3E)

These previously stable APIs are now stable in const contexts:

- [`<[T; N]>::each_ref`](https://doc.rust-lang.org/stable/std/primitive.array.html#method.each_ref)
- [`<[T; N]>::each_mut`](https://doc.rust-lang.org/stable/std/primitive.array.html#method.each_mut)
- [`OsString::new`](https://doc.rust-lang.org/stable/std/ffi/struct.OsString.html#method.new)
- [`PathBuf::new`](https://doc.rust-lang.org/stable/std/path/struct.PathBuf.html#method.new)
- [`TypeId::of`](https://doc.rust-lang.org/stable/std/any/struct.TypeId.html#method.of)
- [`ptr::with_exposed_provenance`](https://doc.rust-lang.org/stable/std/ptr/fn.with_exposed_provenance.html)
- [`ptr::with_exposed_provenance_mut`](https://doc.rust-lang.org/stable/std/ptr/fn.with_exposed_provenance_mut.html)

<a id="1.91.0-Cargo"></a>

## Cargo

- 🎉 Stabilize `build.build-dir`.
  This config sets the directory where intermediate build artifacts are stored.
  These artifacts are produced by Cargo and rustc during the build process.
  End users usually won't need to interact with them, and the layout inside
  `build-dir` is an implementation detail that may change without notice.
  ([config doc](https://doc.rust-lang.org/stable/cargo/reference/config.html#buildbuild-dir))
  ([build cache doc](https://doc.rust-lang.org/stable/cargo/reference/build-cache.html))
  [#&#8203;15833](https://redirect.github.com/rust-lang/cargo/pull/15833)
  [#&#8203;15840](https://redirect.github.com/rust-lang/cargo/pull/15840)
- The `--target` flag and the `build.target` configuration can now take literal
  `"host-tuple"` string, which will internally be substituted by the host
  machine's target triple.
  [#&#8203;15838](https://redirect.github.com/rust-lang/cargo/pull/15838)
  [#&#8203;16003](https://redirect.github.com/rust-lang/cargo/pull/16003)
  [#&#8203;16032](https://redirect.github.com/rust-lang/cargo/pull/16032)

<a id="1.91.0-Rustdoc"></a>

## Rustdoc

- [In search results, rank doc aliases lower than non-alias items with the same name](https://redirect.github.com/rust-lang/rust/pull/145100)
- [Raw pointers now work in type-based search like references](https://redirect.github.com/rust-lang/rust/pull/145731). This means you can now search for things like `*const u8 ->`, and additionally functions that take or return raw pointers will now display their signature properly in search results.

<a id="1.91.0-Compatibility-Notes"></a>

## Compatibility Notes

- [Always require coroutine captures to be drop-live](https://redirect.github.com/rust-lang/rust/pull/144156)
- [Apple: Always pass SDK root when linking with `cc`, and pass it via `SDKROOT` env var](https://redirect.github.com/rust-lang/rust/pull/131477). This should fix linking issues with `rustc` running inside Xcode. Libraries in `/usr/local/lib` may no longer be linked automatically, if you develop or use a crate that relies on this, you should explicitly set `cargo::rustc-link-search=/usr/local/lib` in a `build.rs` script.
- [Relaxed bounds in associated type bound position like in `TraitRef<AssocTy: ?Sized>` are now correctly forbidden](https://redirect.github.com/rust-lang/rust/pull/135331)
- [Add unstable `#[sanitize(xyz = "on|off")]` built-in attribute that shadows procedural macros with the same name](https://redirect.github.com/rust-lang/rust/pull/142681)
- [Fix the drop checker being more permissive for bindings declared with let-else](https://redirect.github.com/rust-lang/rust/pull/143028)
- [Be more strict when parsing attributes, erroring on many invalid attributes](https://redirect.github.com/rust-lang/rust/pull/144689)
  - [Error on invalid `#[should_panic]` attributes](https://redirect.github.com/rust-lang/rust/pull/143808)
  - [Error on invalid `#[link]` attributes](https://redirect.github.com/rust-lang/rust/pull/143193)
- [Mark all deprecation lints in name resolution as deny-by-default and also report in dependencies](https://redirect.github.com/rust-lang/rust/pull/143929)
- The lint `semicolon_in_expressions_from_macros`, for `macro_rules!` macros in expression position that expand to end in a semicolon (`;`), is now deny-by-default. It was already warn-by-default, and a future compatibility warning (FCW) that warned even in dependencies. This lint will become a hard error in the future.
- [Trait impl modifiers (e.g., `unsafe`, `!`, `default`) in inherent impls are no longer syntactically valid](https://redirect.github.com/rust-lang/rust/pull/144386)
- [Start reporting future breakage for `ill_formed_attribute_input` in dependencies](https://redirect.github.com/rust-lang/rust/pull/144544)
- [Restrict the scope of temporaries created by the macros `pin!`, `format_args!`, `write!`, and `writeln!` in `if let` scrutinees in Rust Edition 2024.](https://redirect.github.com/rust-lang/rust/pull/145342) This applies [Rust Edition 2024's `if let` temporary scope rules](https://doc.rust-lang.org/edition-guide/rust-2024/temporary-if-let-scope.html) to these temporaries, which previously could live past the `if` expression regardless of Edition.
- [Invalid numeric literal suffixes in tuple indexing, tuple struct indexing, and struct field name positions are now correctly rejected](https://redirect.github.com/rust-lang/rust/pull/145463)
- [Closures marked with the keyword `static` are now syntactically invalid](https://redirect.github.com/rust-lang/rust/pull/145604)
- [Shebangs inside `--cfg` and `--check-cfg` arguments are no longer allowed](https://redirect.github.com/rust-lang/rust/pull/146211)
- [Add future incompatibility lint for temporary lifetime shortening in Rust 1.92](https://redirect.github.com/rust-lang/rust/pull/147056)

Cargo compatibility notes:

- `cargo publish` no longer keeps `.crate` tarballs as final build artifacts
  when `build.build-dir` is set. These tarballs were previously included due to
  an oversight and are now treated as intermediate artifacts.
  To get `.crate` tarballs as final artifacts, use `cargo package`.
  In a future version, this change will apply regardless of `build.build-dir`.
  [#&#8203;15910](https://redirect.github.com/rust-lang/cargo/pull/15910)
- Adjust Cargo messages to match rustc diagnostic style.
  This changes some of the terminal colors used by Cargo messages.
  [#&#8203;15928](https://redirect.github.com/rust-lang/cargo/pull/15928)
- Tools and projects relying on the
  [internal details of Cargo's `build-dir`](https://doc.rust-lang.org/cargo/reference/build-cache.html)
  may not work for users changing their `build-dir` layout.
  For those doing so, we'd recommend proactively testing these cases
  particularly as we are considering changing the default location of the `build-dir` in the future
  ([cargo#16147](https://redirect.github.com/rust-lang/cargo/issues/16147)).
  If you can't migrate off of Cargo's internal details,
  we'd like to learn more about your use case as we prepare to change the layout of the `build-dir`
  ([cargo#15010](https://redirect.github.com/rust-lang/cargo/issues/15010)).

<a id="1.91.0-Internal-Changes"></a>

## Internal Changes

These changes do not affect any public interfaces of Rust, but they represent
significant improvements to the performance or internals of rustc and related
tools.

- [Update to LLVM 21](https://redirect.github.com/rust-lang/rust/pull/143684)

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNTkuNCIsInVwZGF0ZWRJblZlciI6IjQxLjE1OS40IiwidGFyZ2V0QnJhbmNoIjoibWFzdGVyIiwibGFiZWxzIjpbXX0=-->


---
