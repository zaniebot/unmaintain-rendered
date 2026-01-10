---
number: 949
title: "AppSettings::DeriveDisplayOrder not ordering options by defined order."
type: issue
state: closed
author: Dragonrun1
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2017-05-08T20:04:54Z
updated_at: 2018-08-02T03:30:06Z
url: https://github.com/clap-rs/clap/issues/949
synced_at: 2026-01-10T01:26:39Z
---

# AppSettings::DeriveDisplayOrder not ordering options by defined order.

---

_Issue opened by @Dragonrun1 on 2017-05-08 20:04_

### Rust Version

rustc 1.19.0-nightly (f4209651e 2017-05-05)

### Affected Version of clap

clap 2.24.1

### Expected Behavior Summary

Options and flag appear in same order as they are defined.

### Actual Behavior Summary

Some options are moved into a seemingly more alphabetical order instead.
Good example the '-d' option is moved before the '-n' option even though they are define in opposite order. Another is '--ini' moves up several places instead of just before the '--r*' ones where it should be so it seems to be effecting both long and short options.



### Steps to Reproduce the issue
[Download](https://pastebin.com/cePQwBXc)
Run cargo run --bin test -- -h where 'test' is the downloaded code.
Notice things are not in the defined order.

[Example output](https://pastebin.com/MVNjPd9v)


---

_Comment by @kbknapp on 2017-05-08 22:12_

This is because you don't have [`AppSettings::UnifiedHelpMessage`](https://docs.rs/clap/2.24.1/clap/enum.AppSettings.html#variant.UnifiedHelpMessage) set. 

I understand you're using the `{unified}` in the template. Without getting into the weeds, what happens is clap orders flags and options separately (unless `UnifiedHelpMessage` is set, in which case it merges them at declaration time). Then when printing the help message, if `{unified}` is used *without* having also used `UnifiedHelpMessage` it will do a "best effort" merge.

I'm going to mark this as a documentation issue, as I should probably mention this edge case explicitly.

---

_Label `C: docs` added by @kbknapp on 2017-05-08 22:12_

---

_Label `C: settings` added by @kbknapp on 2017-05-08 22:12_

---

_Label `D: easy` added by @kbknapp on 2017-05-08 22:12_

---

_Label `P4: nice to have` added by @kbknapp on 2017-05-08 22:12_

---

_Label `T: enhancement` added by @kbknapp on 2017-05-08 22:12_

---

_Label `W: 2.x` added by @kbknapp on 2017-05-08 22:12_

---

_Added to milestone `2.24.2` by @kbknapp on 2017-05-09 19:01_

---

_Comment by @Dragonrun1 on 2017-05-10 17:59_

Okay adding the other setting did the trick thanks. Yeah probably a good idea to document it so you don't end up with another bug report down the road from someone else :)

---

_Closed by @kbknapp on 2017-05-16 11:23_

---
