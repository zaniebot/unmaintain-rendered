---
number: 426
title: Allow adding multiple groups from single Arg
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2016-02-15T11:25:19Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/426
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow adding multiple groups from single Arg

---

_Issue opened by @kbknapp on 2016-02-15 11:25_

``` rust
Arg::with_name("arg")
    .groups(&["grp1", "grp2"])
```

I'm willing to mentor anyone who wants to take a stab at this. Things that probably need to happen:
- [ ] Follow [`Arg::requires_all`](https://github.com/kbknapp/clap-rs/blob/e08fdfb08a888d2d32024046525ae07d0c461716/src/args/arg.rs#L592-L601) style method (or any other `_all` type method
- [ ] Change [`Arg.group`](https://github.com/kbknapp/clap-rs/blob/e08fdfb08a888d2d32024046525ae07d0c461716/src/args/arg.rs#L52) from `Option<&'a str>` to `Option<Vec<&'a str>>`
- [ ] Change the relevant code in [`src/app/parser.rs`](https://github.com/kbknapp/clap-rs/blob/e08fdfb08a888d2d32024046525ae07d0c461716/src/app/parser.rs#L121-L124) to accept multiple groups instead of just the one.


---

_Label `T: enhancement` added by @kbknapp on 2016-02-15 11:25_

---

_Label `P4: nice to have` added by @kbknapp on 2016-02-15 11:25_

---

_Label `C: args` added by @kbknapp on 2016-02-15 11:25_

---

_Label `D: easy` added by @kbknapp on 2016-02-15 11:25_

---

_Label `W: 2.x` added by @kbknapp on 2016-02-15 11:25_

---

_Label `M: mentored` added by @kbknapp on 2016-02-15 11:25_

---

_Referenced in [clap-rs/clap#542](../../clap-rs/clap/pulls/542.md) on 2016-06-24 15:55_

---

_Closed by @homu on 2016-06-24 18:43_

---
