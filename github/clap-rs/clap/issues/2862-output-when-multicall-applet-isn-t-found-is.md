---
number: 2862
title: "Output when multicall applet isn't found is unhelpful and ugly"
type: issue
state: closed
author: fishface60
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-mentor
assignees: []
created_at: 2021-10-12T20:05:13Z
updated_at: 2022-05-03T21:45:53Z
url: https://github.com/clap-rs/clap/issues/2862
synced_at: 2026-01-07T13:12:19-06:00
---

# Output when multicall applet isn't found is unhelpful and ugly

---

_Issue opened by @fishface60 on 2021-10-12 20:05_

### Affected Version of clap

* 3.0.0-beta.4

### Expected Behavior Summary

When I run a multicall program from a link that does not match an applet, I expect an error message explaining that the applet was not found and which applets exist.

### Actual Behavior Summary

Error message just lists that the argument wasn't expected.

### Steps to Reproduce the issue

```
$ cargo build --examples
$ ln target/debug/examples/24_multicall_busybox asdf
$ ./asdf 
error: Found argument 'asdf' which wasn't expected, or isn't valid in this context

USAGE:
     [SUBCOMMAND]

For more information try --help
```

---

_Label `T: enhancement` added by @pksunkara on 2021-10-12 20:06_

---

_Label `C: errors` added by @pksunkara on 2021-10-12 20:06_

---

_Referenced in [clap-rs/clap#2861](../../clap-rs/clap/issues/2861.md) on 2021-10-12 20:06_

---

_Referenced in [epage/clapng#215](../../epage/clapng/issues/215.md) on 2021-12-06 22:18_

---

_Referenced in [epage/clapng#216](../../epage/clapng/issues/216.md) on 2021-12-06 22:18_

---

_Referenced in [clap-rs/clap#3041](../../clap-rs/clap/pulls/3041.md) on 2021-12-08 19:29_

---

_Label `C: errors` removed by @epage on 2021-12-08 20:24_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:24_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 21:29_

---

_Referenced in [clap-rs/clap#3677](../../clap-rs/clap/pulls/3677.md) on 2022-05-02 17:08_

---

_Closed by @epage on 2022-05-03 21:45_

---
