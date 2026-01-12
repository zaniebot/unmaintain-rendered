```yaml
number: 1231
title: "No way to set `required(false)` on `@group` in `clap_app!`"
type: issue
state: closed
author: LegNeato
labels:
  - C-bug
assignees: []
created_at: 2018-03-25T07:35:49Z
updated_at: 2018-08-02T03:30:21Z
url: https://github.com/clap-rs/clap/issues/1231
synced_at: 2026-01-12T16:14:10Z
```

# No way to set `required(false)` on `@group` in `clap_app!`

---

_@LegNeato_


### Rust Version

* rustc 1.26.0-nightly (f5631d9ac 2018-03-24)

### Affected Version of clap

* 2.31.1

### Expected Behavior Summary

Have `required(false)` set on the `ArgGroup`  when using the `clap_app!()` macro

### Actual Behavior Summary

Error thrown:

`no rules expected the token 'app'`

### Steps to Reproduce the issue

Given something like this 
```
clap_app!(
        date =>
            (@group dates !required =>      // NOTE the `!required`
             (@arg date: -d --date [STRING]
              "display time described by STRING, not 'now'")
             (@arg file: -f --file [DATEFILE]
              "like --date; once for each line of DATEFILE"))
             })
```
See that it throws an error. It appears the macro only supports `!required` in `(@arg)` declarations and not in `(@group)`.

I also tried `(@attributes !required)` in the group, and while that parsed without throwing an error it had no effect.


---

_Renamed from "No way to set `required(false)` on `@group` in `clap_app!()`" to "No way to set `required(false)` on `@group` in `clap_app!`" by @LegNeato on 2018-03-25 07:36_

---

_Comment by @kbknapp on 2018-03-26 00:56_

Thanks for posting, I'll look into this as it's probably just a simple omission :wink: 

---

_Label `T: bug` added by @kbknapp on 2018-03-26 00:56_

---

_Label `P2: need to have` added by @kbknapp on 2018-03-26 00:56_

---

_Label `D: easy` added by @kbknapp on 2018-03-26 00:56_

---

_Label `W: 2.x` added by @kbknapp on 2018-03-26 00:56_

---

_Label `C: macros` added by @kbknapp on 2018-03-26 00:56_

---

_Referenced in [clap-rs/clap#1238](../../clap-rs/clap/pulls/1238.md) on 2018-03-30 20:56_

---

_Closed by @kbknapp on 2018-04-01 03:30_

---

_Referenced in [clap-rs/clap#2248](../../clap-rs/clap/pulls/2248.md) on 2020-12-10 07:43_

---
