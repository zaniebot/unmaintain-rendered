---
number: 1329
title: Rust 2018 support
type: issue
state: closed
author: royaldark
labels: []
assignees: []
created_at: 2018-08-08T04:44:48Z
updated_at: 2019-03-26T08:59:00Z
url: https://github.com/clap-rs/clap/issues/1329
synced_at: 2026-01-07T13:12:19-06:00
---

# Rust 2018 support

---

_Issue opened by @royaldark on 2018-08-08 04:44_

Hey all! I've been using the Rust 2018 preview in recent projects. It isn't out officially for another couple months, but soon it will supplant Rust 2015.

I've noticed at least one (minor) issue in testing using Clap in Rust 2018.

In Rust 2018, macro imports can be individually imported via `use`. However, importing the `arg_enum` macro seems to require another private macro.

```rust
use clap::arg_enum;

arg_enum!{
    #[derive(Debug)]
    pub enum ArgEnum {
        ThingA,
        ThingB
    }
}
```

```
error: cannot find macro `_clap_count_exprs!` in this scope
 --> src\enums.rs:3:1
  |
3 | / arg_enum!{
4 | |     #[derive(Debug)]
5 | |     pub enum ArgEnum {
6 | |         ThingA,
7 | |         ThingB
8 | |     }
9 | | }
  | |_^
  |
  = note: this error originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)
```

This can be worked around by additionally importing `clap::_clap_count_exprs`.

### Rust Version
rustc 1.29.0-nightly (6a1c0637c 2018-07-23)

all needed [Rust 2018 changes](https://rust-lang-nursery.github.io/edition-guide/editions/transitioning.html) applied

### Affected Version of clap

2.23.0



---

_Referenced in [clap-rs/clap#1397](../../clap-rs/clap/pulls/1397.md) on 2018-12-18 23:04_

---

_Comment by @ErichDonGubler on 2018-12-18 23:06_

@royaldark: Looks like your Rust 2018 link rotted to this: https://rust-lang-nursery.github.io/edition-guide/editions/transitioning-an-existing-project-to-a-new-edition.html

---

_Comment by @NeoLegends on 2018-12-28 15:38_

The same occurs when trying to use the `app_from_crate`-macro, which depends on a handful of others that aren't resolved properly at the moment (at least when using Rust 2018).

---

_Comment by @killercup on 2019-01-03 15:32_

Hm, I fear this is not easily fixable while still [supporting](https://github.com/clap-rs/clap/blob/3294d18efe5f264d12c9035f404c7d189d4824e1/.travis.yml#L9) Rust 1.21. One workaround I can see is using a build script that detects the Rust version and set a feature dependent on that.

---

_Comment by @ErichDonGubler on 2019-01-03 15:59_

@killercup: The [minimum Rust version policy of this crate](https://github.com/clap-rs/clap#minimum-version-of-rust) is to support `$latest - 2` releases. Why is 1.21 in the scope of the conversation as opposed to Rust 1.29?

---

_Comment by @killercup on 2019-01-03 16:17_

@ErichDonGubler oh, I just looked at the CI config, sorry! Then we should be good to go ahead with #1397. Sorry for the confusion!

---

_Comment by @ErichDonGubler on 2019-01-03 22:21_

@killercup: No problem! :) I wish I could just use something like a reminder bot here for Monday...

---

_Comment by @mkpankov on 2019-02-01 12:06_

Just stumbled onto this, would appreciate a fix.

---

_Comment by @ErichDonGubler on 2019-02-01 15:53_

Pinging here for somebody to look at https://github.com/clap-rs/clap/pull/1397, please! I'm the author of that PR, and I'm more than happy to do whatever is necessary to get it over the line. :)

---

_Closed by @spacekookie on 2019-03-26 08:59_

---
