```yaml
number: 17523
title: Update Rust to v1.92.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/rust-1.x
created_at: 2026-01-16T14:25:19Z
updated_at: 2026-01-16T17:14:36Z
url: https://github.com/astral-sh/uv/pull/17523
synced_at: 2026-01-16T18:01:14Z
```

# Update Rust to v1.92.0

---

_@renovate_

This PR contains the following updates:

| Package | Update | Change |
|---|---|---|
| [rust](https://redirect.github.com/rust-lang/rust) | minor | `1.91` → `1.92.0` |

---

### Release Notes

<details>
<summary>rust-lang/rust (rust)</summary>

### [`v1.92.0`](https://redirect.github.com/rust-lang/rust/blob/HEAD/RELEASES.md#Version-1920-2025-12-11)

[Compare Source](https://redirect.github.com/rust-lang/rust/compare/1.91.1...1.92.0)

\==========================

<a id="1.92.0-Language"></a>

## Language

- [Document `MaybeUninit` representation and validity](https://redirect.github.com/rust-lang/rust/pull/140463)
- [Allow `&raw [mut | const]` for union field in safe code](https://redirect.github.com/rust-lang/rust/pull/141469)
- [Prefer item bounds of associated types over where-bounds for auto-traits and `Sized`](https://redirect.github.com/rust-lang/rust/pull/144064)
- [Do not materialize `X` in `[X; 0]` when `X` is unsizing a const](https://redirect.github.com/rust-lang/rust/pull/145277)
- [Support combining `#[track_caller]` and `#[no_mangle]` (requires every declaration specifying `#[track_caller]` as well)](https://redirect.github.com/rust-lang/rust/pull/145724)
- [Make never type lints `never_type_fallback_flowing_into_unsafe` and `dependency_on_unit_never_type_fallback` deny-by-default](https://redirect.github.com/rust-lang/rust/pull/146167)
- [Allow specifying multiple bounds for same associated item, except in trait objects](https://redirect.github.com/rust-lang/rust/pull/146593)
- [Slightly strengthen higher-ranked region handling in coherence](https://redirect.github.com/rust-lang/rust/pull/146725)
- [The `unused_must_use` lint no longer warns on `Result<(), Uninhabited>` (for instance, `Result<(), !>`), or `ControlFlow<Uninhabited, ()>`](https://redirect.github.com/rust-lang/rust/pull/147382). This avoids having to check for an error that can never happen.

<a id="1.92.0-Compiler"></a>

## Compiler

- [Make `mips64el-unknown-linux-muslabi64` link dynamically](https://redirect.github.com/rust-lang/rust/pull/146858)
- [Remove current code for embedding command-line args in PDB](https://redirect.github.com/rust-lang/rust/pull/147022)
  Command-line information is typically not needed by debugging tools, and the removed code
  was causing problems for incremental builds even on targets that don't use PDB debuginfo.

<a id="1.92.0-Libraries"></a>

## Libraries

- [Specialize `Iterator::eq{_by}` for `TrustedLen` iterators](https://redirect.github.com/rust-lang/rust/pull/137122)
- [Simplify `Extend` for tuples](https://redirect.github.com/rust-lang/rust/pull/138799)
- [Added details to `Debug` for `EncodeWide`](https://redirect.github.com/rust-lang/rust/pull/140153).
- [`iter::Repeat::last`](https://redirect.github.com/rust-lang/rust/pull/147258) and [`count`](https://redirect.github.com/rust-lang/rust/pull/146410) will now panic, rather than looping infinitely.

<a id="1.92.0-Stabilized-APIs"></a>

## Stabilized APIs

- [`NonZero<u{N}>::div_ceil`](https://doc.rust-lang.org/stable/std/num/struct.NonZero.html#method.div_ceil)
- [`Location::file_as_c_str`](https://doc.rust-lang.org/stable/std/panic/struct.Location.html#method.file_as_c_str)
- [`RwLockWriteGuard::downgrade`](https://doc.rust-lang.org/stable/std/sync/struct.RwLockWriteGuard.html#method.downgrade)
- [`Box::new_zeroed`](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html#method.new_zeroed)
- [`Box::new_zeroed_slice`](https://doc.rust-lang.org/stable/std/boxed/struct.Box.html#method.new_zeroed_slice)
- [`Rc::new_zeroed`](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html#method.new_zeroed)
- [`Rc::new_zeroed_slice`](https://doc.rust-lang.org/stable/std/rc/struct.Rc.html#method.new_zeroed_slice)
- [`Arc::new_zeroed`](https://doc.rust-lang.org/stable/std/sync/struct.Arc.html#method.new_zeroed)
- [`Arc::new_zeroed_slice`](https://doc.rust-lang.org/stable/std/sync/struct.Arc.html#method.new_zeroed_slice)
- [`btree_map::Entry::insert_entry`](https://doc.rust-lang.org/stable/std/collections/btree_map/enum.Entry.html#method.insert_entry)
- [`btree_map::VacantEntry::insert_entry`](https://doc.rust-lang.org/stable/std/collections/btree_map/struct.VacantEntry.html#method.insert_entry)
- [`impl Extend<proc_macro::Group> for proc_macro::TokenStream`](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html#impl-Extend%3CGroup%3E-for-TokenStream)
- [`impl Extend<proc_macro::Literal> for proc_macro::TokenStream`](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html#impl-Extend%3CLiteral%3E-for-TokenStream)
- [`impl Extend<proc_macro::Punct> for proc_macro::TokenStream`](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html#impl-Extend%3CPunct%3E-for-TokenStream)
- [`impl Extend<proc_macro::Ident> for proc_macro::TokenStream`](https://doc.rust-lang.org/stable/proc_macro/struct.TokenStream.html#impl-Extend%3CIdent%3E-for-TokenStream)

These previously stable APIs are now stable in const contexts:

- [`<[_]>::rotate_left`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.rotate_left)
- [`<[_]>::rotate_right`](https://doc.rust-lang.org/stable/std/primitive.slice.html#method.rotate_right)

<a id="1.92.0-Cargo"></a>

## Cargo

- [Added a new chapter](https://redirect.github.com/rust-lang/cargo/issues/16119) to the Cargo book, ["Optimizing Build Performance"](https://doc.rust-lang.org/stable/cargo/guide/build-performance.html).

<a id="1.92.0-Rustdoc"></a>

## Rustdoc

- [If a trait item appears in rustdoc search, hide the corresponding impl items](https://redirect.github.com/rust-lang/rust/pull/145898).  Previously a search for "last" would show both `Iterator::last` as well as impl methods like `std::vec::IntoIter::last`.  Now these impl methods will be hidden, freeing up space for inherent methods like `BTreeSet::last`.
- [Relax rules for identifiers in search](https://redirect.github.com/rust-lang/rust/pull/147860).  Previously you could only search for identifiers that were valid in rust code, now searches only need to be valid as part of an identifier.  For example, you can now perform a search that starts with a digit.

<a id="1.92.0-Compatibility-Notes"></a>

## Compatibility Notes

- [Fix backtraces with `-C panic=abort` on Linux by generating unwind tables by default](https://redirect.github.com/rust-lang/rust/pull/143613). Build with `-C force-unwind-tables=no` to keep omitting unwind tables.

* As part of the larger effort refactoring compiler built-in attributes and their diagnostics, [the future-compatibility lint `invalid_macro_export_arguments` is upgraded to deny-by-default and will be reported in dependencies too.](https://redirect.github.com/rust-lang/rust/pull/143857)
* [Update the minimum external LLVM to 20](https://redirect.github.com/rust-lang/rust/pull/145071)
* [Prevent downstream `impl DerefMut for Pin<LocalType>`](https://redirect.github.com/rust-lang/rust/pull/145608)
* [Don't apply temporary lifetime extension rules to the arguments of non-extended `pin!` and formatting macros](https://redirect.github.com/rust-lang/rust/pull/145838)

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-16 14:25_

---

_Comment by @zanieb on 2026-01-16 17:01_

cc @konstin I had to make some changes here

---

_@zanieb reviewed on 2026-01-16 17:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/diagnostics.rs`:337 on 2026-01-16 17:01_

I'm also picking back up removal of miette

---

_Review comment by @konstin on `crates/uv/src/commands/diagnostics.rs`:337 on 2026-01-16 17:03_

I have a pitch for an alternative to https://github.com/zkat/miette/pull/459 and/or we can fork if we have to, but removal of miette but imho be better long term, one less pattern in error handling.

---

_@konstin reviewed on 2026-01-16 17:03_

---

_Review comment by @zanieb on `crates/uv/src/commands/diagnostics.rs`:337 on 2026-01-16 17:07_

I'm just going to remove it yeah. I just forgot about #17110 and need to do a horrid rebase.

---

_@zanieb reviewed on 2026-01-16 17:07_

---

_Merged by @zanieb on 2026-01-16 17:14_

---

_Closed by @zanieb on 2026-01-16 17:14_

---

_Branch deleted on 2026-01-16 17:14_

---
