---
number: 5238
title: Unresolved Links in clap Crate Documentation
type: issue
state: closed
author: lpnh
labels: []
assignees: []
created_at: 2023-12-02T13:13:10Z
updated_at: 2023-12-02T14:10:51Z
url: https://github.com/clap-rs/clap/issues/5238
synced_at: 2026-01-10T01:28:08Z
---

# Unresolved Links in clap Crate Documentation

---

_Issue opened by @lpnh on 2023-12-02 13:13_

## Summary

While working on a project that uses the `clap` crate, I encountered a few broken links in the documentation preview when hovering over the crate in my IDE. To investigate further, I cloned the repository and ran `cargo doc`, which revealed several unresolved link warnings.

## Steps to Reproduce

Clone the `clap` repository.
Navigate to the `clap` directory.
Run `cargo doc`.
Observe the following warning messages indicating unresolved links:

```
warning: unresolved link to `super::Arg::env`
   --> clap_builder/src/builder/action.rs:344:68
    |
344 |     /// [`default_values`][super::Arg::default_values] and [`env`][super::Arg::env] may still be
    |                                                                    ^^^^^^^^^^^^^^^ the struct `Arg` has no field or associated item named `env`
    |
    = note: `#[warn(rustdoc::broken_intra_doc_links)]` on by default

warning: unresolved link to `crate::crate_name`
   --> clap_builder/src/builder/command.rs:120:68
    |
120 |     /// See also [`command!`](crate::command!) and [`crate_name!`](crate::crate_name!).
    |                                                                    ^^^^^^^^^^^^^^^^^^ no item named `crate_name` in module `clap_builder`

warning: unresolved link to `crate_authors`
    --> clap_builder/src/builder/command.rs:1656:54
     |
1656 |     /// **Pro-tip:** Use `clap`s convenience macro [`crate_authors!`] to
     |                                                      ^^^^^^^^^^^^^^ no item named `crate_authors` in scope
     |
     = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `crate::crate_description`
    --> clap_builder/src/builder/command.rs:1685:41
     |
1685 |     /// See also [`crate_description!`](crate::crate_description!).
     |                                         ^^^^^^^^^^^^^^^^^^^^^^^^^ no item named `crate_description` in module `clap_builder`

warning: unresolved link to `crate_version`
    --> clap_builder/src/builder/command.rs:1820:54
     |
1820 |     /// **Pro-tip:** Use `clap`s convenience macro [`crate_version!`] to
     |                                                      ^^^^^^^^^^^^^^ no item named `crate_version` in scope
     |
     = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `crate_version`
    --> clap_builder/src/builder/command.rs:1843:54
     |
1843 |     /// **Pro-tip:** Use `clap`s convenience macro [`crate_version!`] to
     |                                                      ^^^^^^^^^^^^^^ no item named `crate_version` in scope
     |
     = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `crate::Arg::env`
 --> clap_builder/src/parser/matches/value_source.rs:7:33
  |
7 |     /// Value came [`Arg::env`][crate::Arg::env]
  |                                 ^^^^^^^^^^^^^^^ the struct `Arg` has no field or associated item named `env`

    Checking clap v4.4.10 (/user/clap)
warning: `clap_builder` (lib doc) generated 7 warnings
 Documenting clap v4.4.10 (/user/clap)
warning: unresolved link to `_derive::_tutorial::chapter_0`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   = note: no item named `_derive` in scope
   = note: `#[warn(rustdoc::broken_intra_doc_links)]` on by default

warning: unresolved link to `_derive`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
                                                                              ^^^^^^^
   = note: no item named `_derive` in scope
   = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `_tutorial::chapter_0`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           - Builder [tutorial][_tutorial::chapter_0] and [reference](index.html)
                                ^^^^^^^^^^^^^^^^^^^^
   = note: no item named `_tutorial` in scope

warning: unresolved link to `_cookbook`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           - [Cookbook][_cookbook]
                        ^^^^^^^^^
   = note: no item named `_cookbook` in scope
   = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `_faq`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           - [FAQ][_faq]
                   ^^^^
   = note: no item named `_faq` in scope
   = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `_features`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           *(See also [feature flag reference][_features])*
                                               ^^^^^^^^^
   = note: no item named `_features` in scope
   = help: to escape `[` and `]` characters, add '\' before them like `\[` or `\]`

warning: unresolved link to `_derive::_tutorial`
  --> src/lib.rs:6:1
   |
6  | / //! > **Command Line Argument Parser for Rust**
7  | | //!
8  | | //! Quick Links:
9  | | //! - Derive [tutorial][_derive::_tutorial::chapter_0] and [reference][_derive]
...  |
78 | | //! - [Command-line Apps for Rust](https://rust-cli.github.io/book/index.html) book
79 | | //!
   | |___^
   |
   = note: the link appears in this line:
           
           See also the derive [tutorial][_derive::_tutorial] and [reference][_derive]
                                          ^^^^^^^^^^^^^^^^^^
   = note: no item named `_derive` in scope

warning: `clap` (lib doc) generated 7 warnings
```

## Partial Solution

In an attempt to resolve these warnings, I discovered that removing the `#[cfg(feature = "unstable-doc")]` attribute from [lib.rs](https://github.com/clap-rs/clap/blob/c0a1814d3c1d93c18faaedc95fd251846e47f4fe/src/lib.rs#L106) eliminates most of them. However, I'm not entirely sure if this is the appropriate solution, as it might have other unintended effects on the documentation or crate functionality.

---

_Comment by @epage on 2023-12-02 14:10_

All of those links are for functionality behind feature flags and itd be expected for them to not work without those features enabledd

---

_Closed by @epage on 2023-12-02 14:10_

---
