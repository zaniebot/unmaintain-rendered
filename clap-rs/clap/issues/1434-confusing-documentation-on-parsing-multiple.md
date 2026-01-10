---
number: 1434
title: confusing documentation on parsing multiple subcommands (minor issue)
type: issue
state: closed
author: 0x900C5649
labels:
  - C-enhancement
  - A-help
  - A-docs
assignees: []
created_at: 2019-03-20T12:14:41Z
updated_at: 2021-08-13T19:52:42Z
url: https://github.com/clap-rs/clap/issues/1434
synced_at: 2026-01-10T01:26:54Z
---

# confusing documentation on parsing multiple subcommands (minor issue)

---

_Issue opened by @0x900C5649 on 2019-03-20 12:14_

I am working on a personal project for which I would like to parse a list of subcommands (together with their arguments).
So something like:
```bash
./myprogram generatelist --size 12 storelist mylist.txt
```
I wanted this to give me two subcommands (generatelist and storelist) with their arguments.

I thought this is possible because the [documentation] (https://docs.rs/clap/2.32.0/clap/enum.AppSettings.html#variant.ArgsNegateSubcommands)  on `AppSettings::ArgsNegateSubcommands` states:

> By default `clap` allows arguments between subcommands such as `<cmd> [cmd_args] <cmd2> [cmd2_args] <cmd3> [cmd3_args]`

As I couldn't make my program work I checked for more sources and found
- #84 (which is marked as `W: wont do`)
- The comment in `clap/examples/20_subcommands.rs`:
  > ```rust
  > // Notice only one command per "level" may be used. You could not, for example, do:
  > //
  > // $ git clone url push origin path
  > ```
Both basically state, that what I wanted to do is not possible.

So my best guess is, that the documentation on `AppSettings::ArgsNegateSubcommands` assumes `<cmd3>` to be a subcommand of `<cmd2>`, which should be a subcommand of `<cmd>`

### Possible fix
The simplest solution to avoid this confusion would probably be to rename the subcommands in the documentation to `<cmd>`, `<subcmd>` and `<subsubcommand>`

---

_Comment by @ithinuel on 2019-03-29 20:57_

I had the exact same "adventure" than you.

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-01 14:43_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 14:43_

---

_Label `P3: want to have` added by @CreepySkeleton on 2020-02-01 14:43_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 14:43_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 14:43_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-02-01 14:43_

---

_Comment by @pksunkara on 2021-05-26 03:17_

The actual issue is #2222 but leaving this issue open for docs fix.

---

_Label `W: 3.x` removed by @pksunkara on 2021-06-21 20:37_

---

_Referenced in [clap-rs/clap#2685](../../clap-rs/clap/pulls/2685.md) on 2021-08-13 18:11_

---

_Closed by @pksunkara on 2021-08-13 19:52_

---
