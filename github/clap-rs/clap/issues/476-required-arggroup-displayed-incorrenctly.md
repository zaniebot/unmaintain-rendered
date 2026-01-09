---
number: 476
title: Required ArgGroup displayed incorrenctly
type: issue
state: closed
author: panicbit
labels:
  - C-bug
assignees: []
created_at: 2016-04-10T00:35:00Z
updated_at: 2018-08-02T03:29:49Z
url: https://github.com/clap-rs/clap/issues/476
synced_at: 2026-01-07T13:12:19-06:00
---

# Required ArgGroup displayed incorrenctly

---

_Issue opened by @panicbit on 2016-04-10 00:35_

The last character in the usage message of `ArgGroup`s seems to always be missing.

For example this code

``` rust
App::new("demo")
    .arg(Arg::with_name("foo").long("foo"))
    .arg(Arg::with_name("bar").long("bar"))
    .group(ArgGroup::with_name("options").required(true).args(&[
        "foo", "bar"
    ]))
    .get_matches();
```

displays the group as `[--foo|--ba]`


---

_Referenced in [clap-rs/clap#477](../../clap-rs/clap/pulls/477.md) on 2016-04-10 00:50_

---

_Comment by @kbknapp on 2016-04-10 05:36_

As I said in the PR, thanks for finding, filing, and fixing this! :+1: 


---

_Referenced in [clap-rs/clap#479](../../clap-rs/clap/pulls/479.md) on 2016-04-10 05:40_

---

_Label `T: bug` added by @kbknapp on 2016-04-10 05:41_

---

_Label `P2: need to have` added by @kbknapp on 2016-04-10 05:41_

---

_Label `D: easy` added by @kbknapp on 2016-04-10 05:41_

---

_Label `W: 2.x` added by @kbknapp on 2016-04-10 05:41_

---

_Label `C: errors` added by @kbknapp on 2016-04-10 05:41_

---

_Label `C: arg groups` added by @kbknapp on 2016-04-10 05:41_

---

_Closed by @homu on 2016-04-11 01:16_

---
