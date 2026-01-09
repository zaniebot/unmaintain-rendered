---
number: 1552
title: "Use of deprecated \"std::sync::ONCE_INIT\""
type: issue
state: closed
author: Zooce
labels: []
assignees: []
created_at: 2019-09-26T19:24:46Z
updated_at: 2019-12-10T20:20:38Z
url: https://github.com/clap-rs/clap/issues/1552
synced_at: 2026-01-07T13:12:19-06:00
---

# Use of deprecated "std::sync::ONCE_INIT"

---

_Issue opened by @Zooce on 2019-09-26 19:24_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version
`rustc 1.38.0 (625451e37 2019-09-23)`

### Affected Version of clap
`clap v2.33.0`

### Bug or Feature Request Summary
Warnings for use of deprecated feature.

### Expected Behavior Summary
No warnings for use  of deprecated feature.

### Actual Behavior Summary
```
warning: use of deprecated item 'std::sync::ONCE_INIT': the `new` function is now preferred
   --> src\settings.rs:151:17
    |
151 |         .author(clap::crate_authors!("\n"))
    |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: `#[warn(deprecated)]` on by default
    = note: this warning originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)

warning: use of deprecated item 'std::sync::ONCE_INIT': the `new` function is now preferred
   --> src\settings.rs:151:17
    |
151 |         .author(clap::crate_authors!("\n"))
    |                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
    |
    = note: this warning originates in a macro outside of the current crate (in Nightly builds, run with -Z external-macro-backtrace for more info)
```

### Steps to Reproduce the issue
Build an app that uses `clap::crate_authors!("\n")` with the versions of `rustc` and `clap` provided above.


---

_Comment by @Zooce on 2019-09-26 19:26_

Similar to #1517

---

_Referenced in [clap-rs/clap#1558](../../clap-rs/clap/pulls/1558.md) on 2019-10-02 13:11_

---

_Closed by @Dylan-DPC-zz on 2019-10-04 12:45_

---

_Comment by @bwinton on 2019-12-10 20:20_

I'm seeing this warning a bunch, and I'm glad that it's fixed, but am wondering if I could ask you to do a release that includes it to calm my compiler down? 😉

(But if a 3.0 beta release is upcoming, I'll happily switch to that instead! 🙂)

---

_Referenced in [alacritty/alacritty#3617](../../alacritty/alacritty/issues/3617.md) on 2020-04-20 07:26_

---

_Referenced in [dmfutcher/git-profile#3](../../dmfutcher/git-profile/pulls/3.md) on 2020-11-11 03:27_

---
