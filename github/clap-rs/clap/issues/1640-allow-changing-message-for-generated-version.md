---
number: 1640
title: Allow changing message for generated version subcommand
type: issue
state: closed
author: casey
labels:
  - A-help
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $5"
assignees: []
created_at: 2020-01-22T01:40:27Z
updated_at: 2021-02-07T18:05:36Z
url: https://github.com/clap-rs/clap/issues/1640
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow changing message for generated version subcommand

---

_Issue opened by @casey on 2020-01-22 01:40_

Is it possible to change the auto-generated help and version sub-command help strings, as is possible with `App::help_message` and `App::version_message`?

And similarly, is it possible to recursively change the auto-generated help and version messages for subcommands?

I like to use the imperative mood for help text (So `Print` and not `Prints`) and I'm looking for a way to change the strings recursively for the whole app.

---

_Added to milestone `v3.0.0-alpha.3` by @CreepySkeleton on 2020-02-01 10:45_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 10:46_

---

_Label `D: easy` added by @CreepySkeleton on 2020-02-01 10:46_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-01 10:46_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 10:46_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 10:46_

---

_Removed from milestone `3.0.0-alpha.3` by @pksunkara on 2020-02-14 14:47_

---

_Added to milestone `3.0` by @pksunkara on 2020-02-14 14:47_

---

_Removed from milestone `3.0` by @pksunkara on 2020-04-09 08:14_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:14_

---

_Comment by @pksunkara on 2020-08-16 08:01_

> And similarly, is it possible to recursively change the auto-generated help and version messages for subcommands?

Can you explain what you mean by this? We have templates, would that solve this?

---

_Comment by @casey on 2020-08-17 00:20_

> Can you explain what you mean by this? We have templates, would that solve this?

Sure, I like to have all my command help messages use the imperative. So I use "Print version number." instead of "Prints version number.".

There are two things I'm having difficulty with:

1. I think currently it isn't possible to change the help text for the automatically generated `help` subcommand. Currently it's "Prints this message or the help of the given subcommand(s)".

2. Since I change all my help and version commands to "Print help message." and "Print version number.", I'd like a way to do this for all subcommands in the app. Currently I do it for each subcommand individually, which isn't a huge problem, but it's easy to forget to do for a new subcommand.

I'm not sure, but I don't think that templates help for this.

---

_Comment by @CreepySkeleton on 2020-08-17 03:35_

He's basically asking for `App::global_help_message` `App::global_version_message`.

---

_Renamed from "Ability to change auto-generated help and version sub-command help strings" to "Add App::global_help_message and global_version_message" by @CreepySkeleton on 2020-08-17 03:36_

---

_Renamed from "Add App::global_help_message and global_version_message" to "Add App::global_help_message and global_version_message that propagate down to subcommands" by @CreepySkeleton on 2020-08-17 03:36_

---

_Comment by @casey on 2020-08-17 03:37_

Should I split 1. into a separate issue? I.e. a way to change generated help subcommand message?

---

_Comment by @CreepySkeleton on 2020-08-17 04:30_

Yeah, I think it's different enough.

---

_Referenced in [clap-rs/clap#2080](../../clap-rs/clap/issues/2080.md) on 2020-08-17 08:17_

---

_Label `T: new feature` added by @pksunkara on 2020-08-17 08:24_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2020-08-17 08:24_

---

_Renamed from "Add App::global_help_message and global_version_message that propagate down to subcommands" to "Allow changing message for generated version subcommand" by @pksunkara on 2020-08-17 08:25_

---

_Comment by @pksunkara on 2020-08-17 08:29_

To close this:

We need the following 2 api's added to `App` along with tests

* `global_version_about`
* `version_about`

---

_Referenced in [clap-rs/clap#2143](../../clap-rs/clap/pulls/2143.md) on 2020-09-25 13:26_

---

_Closed by @bors[bot] on 2020-09-26 07:20_

---

_Closed by @bors[bot] on 2020-09-26 07:20_

---

_Comment by @pksunkara on 2021-02-07 18:05_

I have thought about this more and removed the added `version_about` in favor of doing `app.mut_arg("version", |a| a.about("version info"))`. Setting it once at the top level `App` should be enough.

---

_Referenced in [clap-rs/clap#2500](../../clap-rs/clap/issues/2500.md) on 2021-05-26 19:25_

---
