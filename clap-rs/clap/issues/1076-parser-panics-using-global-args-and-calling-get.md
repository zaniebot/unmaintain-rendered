---
number: 1076
title: "Parser panics using global args and calling `get_matches_from_safe_borrow()` repeatedly"
type: issue
state: closed
author: tomprogrammer
labels: []
assignees: []
created_at: 2017-10-24T11:00:31Z
updated_at: 2018-08-02T03:30:13Z
url: https://github.com/clap-rs/clap/issues/1076
synced_at: 2026-01-10T01:26:43Z
---

# Parser panics using global args and calling `get_matches_from_safe_borrow()` repeatedly

---

_Issue opened by @tomprogrammer on 2017-10-24 11:00_


### Rust Version

rustc 1.21.0 (3b72af97e 2017-10-09)

### Affected Version of clap

v2.26.2

### Expected Behavior Summary

A global argument (`Arg::with_name("vm name").global(true)`) should be usable when calling `get_matches_from_safe_borrow()` multiple times on the same `App`.

### Actual Behavior Summary

The second time clap parses some input it panics and reports "Non-unique argument name: vm name is already in use". (See debug output)

Due to `AppSetting::NoBinaryName` and `get_matches_from_safe_borrow()` available I concluded it would be safe to reuse an `App` instance parsing many input lines. Except for the global arguments other features work well in my REPL-style cli application.

Alternatively I could clone the `App` and reset it before parsing another line of input. Do you consider reusing the `App` instance an API misuse and suggest to use `clone()` and reset, or is this an actual bug that should be fixed?

### Steps to Reproduce the issue

* Clone https://github.com/tomprogrammer/clap-global-issue
* Run `cargo run -- help`
* Type `help` into the REPL
* The error occurs

### Sample Code or Link to Sample Code

https://github.com/tomprogrammer/clap-global-issue

### Debug output

https://gist.github.com/tomprogrammer/4063db64a246b3d2d4bf472cec94fbda


---

_Comment by @kbknapp on 2017-10-24 12:02_

Would you mind testing this against the the current master branch? I just merged #1075 which deals with global args quite heavily and would like to know if that solves the issue.

---

_Comment by @tomprogrammer on 2017-10-24 12:48_

I updated the demonstration repository to use the current master branch. Unfortunately the PR doesn't fix this issue. I also updated the debug log in the gist linked above.

---

_Comment by @kbknapp on 2017-10-24 12:54_

:+1:

---

_Comment by @kbknapp on 2017-10-24 15:40_

I have this fixed in a local branch, I'll upload and put in the PR once I get home today.

---

_Closed by @kbknapp on 2017-10-24 18:59_

---

_Comment by @tomprogrammer on 2017-10-24 19:13_

Thanks for the fast fix and this convenient and powerful crate!

---

_Referenced in [clap-rs/clap#1356](../../clap-rs/clap/issues/1356.md) on 2018-11-10 02:00_

---
