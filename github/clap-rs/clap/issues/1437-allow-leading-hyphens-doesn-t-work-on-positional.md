---
number: 1437
title: "`allow_leading_hyphens` doesn't work on positional arguments"
type: issue
state: closed
author: rocallahan
labels:
  - C-bug
  - E-help-wanted
assignees: []
created_at: 2019-03-26T10:14:28Z
updated_at: 2021-08-08T19:56:09Z
url: https://github.com/clap-rs/clap/issues/1437
synced_at: 2026-01-07T13:12:19-06:00
---

# `allow_leading_hyphens` doesn't work on positional arguments

---

_Issue opened by @rocallahan on 2019-03-26 10:14_

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

`allow_leading_hyphens` doesn't work on positional arguments. Example:

```
use clap::{App, Arg};
fn main() {
    let m = App::new("pattest")
        .arg(Arg::with_name("pat")
            .allow_hyphen_values(true)
            .required(true)
            .takes_value(true))
        .get_matches();
    assert_eq!(m.value_of("pat"), Some("-file"));
}
```
### Expected Behavior Summary

Running with `target/debug/test -file` should exit normally.

### Actual Behavior Summary

```
[roc@localhost test]$ target/debug/test -file
error: Found argument '-f' which wasn't expected, or isn't valid in this context

USAGE:
    test <pat>

For more information try --help
```

---

_Comment by @zicklag on 2020-01-19 00:24_

I'm having this issue in clap 3: commit 92c2b5d.

---

_Label `C: args` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Label `C: positional args` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Label `P3: want to have` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 14:41_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-01 14:41_

---

_Referenced in [getsentry/sentry-cli#770](../../getsentry/sentry-cli/pulls/770.md) on 2020-06-22 15:42_

---

_Comment by @jbangelo on 2021-07-30 21:01_

It won't work for everyone, but [`AppSettings::AllowLeadingHyphen`](https://docs.rs/clap/2.33.1/clap/enum.AppSettings.html#variant.AllowLeadingHyphen) will allow parsing of positional arguments with leading hyphens.

---

_Assigned to @ldm0 by @ldm0 on 2021-08-01 14:26_

---

_Referenced in [uutils/coreutils#2535](../../uutils/coreutils/issues/2535.md) on 2021-08-03 22:14_

---

_Referenced in [clap-rs/clap#2655](../../clap-rs/clap/pulls/2655.md) on 2021-08-07 12:54_

---

_Referenced in [clap-rs/clap#2669](../../clap-rs/clap/pulls/2669.md) on 2021-08-07 19:05_

---

_Closed by @pksunkara on 2021-08-08 19:56_

---
