---
number: 1786
title: Allow abbreviated/unambiguous shortened long options
type: issue
state: closed
author: chungy
labels: []
assignees: []
created_at: 2020-04-04T21:49:05Z
updated_at: 2021-06-06T19:55:10Z
url: https://github.com/clap-rs/clap/issues/1786
synced_at: 2026-01-10T01:27:05Z
---

# Allow abbreviated/unambiguous shortened long options

---

_Issue opened by @chungy on 2020-04-04 21:49_

### Describe your use case

The GNU getopts function in many C programs (especially ones from the GNU project) allows abbreviated versions of long options to be used when the text matches one and only one long option. One such example that I use with extreme frequency is in the `ls` command, `--group-directories-first` can be used simply with `ls --g` since no other long option starts with `g`.

### Describe the solution you'd like

Allowing these options to be shortened in the command line processor. I believe a simple `starts_with` match should work; long options should only be shortened to the shortest unambiguous version. For example, programs with both a `--version` and `--verbose` should not allow `--ver` to select either one.


---

_Label `T: new feature` added by @chungy on 2020-04-04 21:49_

---

_Label `T: new feature` removed by @pksunkara on 2020-04-05 10:44_

---

_Label `C: args` added by @pksunkara on 2020-04-05 10:44_

---

_Label `T: new setting` added by @pksunkara on 2020-04-05 10:44_

---

_Comment by @pksunkara on 2020-04-05 10:45_

We can do something like `InferArgs` which behaves similar to `InferSubcommands`

---

_Added to milestone `3.1` by @pksunkara on 2020-04-05 10:45_

---

_Comment by @Dylan-DPC-zz on 2020-04-07 19:52_

hi @chungy , will you be willing to send in a pull request that adds this feature? thanks

---

_Comment by @chungy on 2020-04-07 20:50_

I'll give a hand at it when I have time, yes.

---

_Referenced in [uutils/coreutils#2350](../../uutils/coreutils/pulls/2350.md) on 2021-06-05 09:35_

---

_Referenced in [clap-rs/clap#2525](../../clap-rs/clap/pulls/2525.md) on 2021-06-05 11:26_

---

_Closed by @pksunkara on 2021-06-06 19:55_

---

_Referenced in [clap-rs/clap#4815](../../clap-rs/clap/issues/4815.md) on 2023-04-14 15:01_

---
