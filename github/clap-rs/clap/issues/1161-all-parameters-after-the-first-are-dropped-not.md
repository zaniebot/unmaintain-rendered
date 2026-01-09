---
number: 1161
title: "All `--` parameters after the first `--` are dropped, not allowing nesting of programs?"
type: issue
state: closed
author: da-x
labels: []
assignees: []
created_at: 2018-02-03T18:31:10Z
updated_at: 2018-08-02T03:30:17Z
url: https://github.com/clap-rs/clap/issues/1161
synced_at: 2026-01-07T13:12:19-06:00
---

# All `--` parameters after the first `--` are dropped, not allowing nesting of programs?

---

_Issue opened by @da-x on 2018-02-03 18:31_

(clap-rs v2.29.2)

Hi,

Suppose I am writing a program that executes another program, then of course it is convenient to take the arguments of the subprogram after `--`,  as illustrated in the `22_stop_parsing_with_--.rs` example. However, if the subprogram itself wants to receive `--`, then `clap-rs` does not seem to currently allow it, and `--` is dropped.

To further explain, if I execute the `22_stop_parsing_with_--.rs` example with the arguments `-f -p=bob -- sloppy slop -a -- subprogram position args` it prints the following:

```
-f used: true
-p's value: Some("bob")
'slops' values: Some(["sloppy", "slop", "-a", "subprogram", "position", "args"])
```

Where did the second `--` go? Can I please have it back somehow?

---

_Referenced in [clap-rs/clap#1162](../../clap-rs/clap/pulls/1162.md) on 2018-02-03 20:02_

---

_Comment by @da-x on 2018-02-03 20:20_

I am currently testing the fix in [pty-for-each](https://github.com/da-x/pty-for-each).

---

_Closed by @kbknapp on 2018-02-04 17:12_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-12 20:40_

---
