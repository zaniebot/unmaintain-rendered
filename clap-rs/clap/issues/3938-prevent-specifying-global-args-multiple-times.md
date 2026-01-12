```yaml
number: 3938
title: Prevent specifying global args multiple times
type: issue
state: open
author: cd-work
labels:
  - C-enhancement
  - M-breaking-change
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2022-07-15T16:11:43Z
updated_at: 2022-11-08T04:30:33Z
url: https://github.com/clap-rs/clap/issues/3938
synced_at: 2026-01-12T16:14:15Z
```

# Prevent specifying global args multiple times

---

_@cd-work_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.8

### Describe your use case

When making an argument global with `Arg::global(true)`, it will be available to both the parent and child subcommand. To me this implies that this option is a single flag that can be modified from anywhere.

However when specifying the global twice (e.g. `cmd --global x subcmd --global y`), the parent's assignment of the variable will be discarded in favor of the subcommand's. While this definitely is a possible way to handle this edgecase, I think this command invocation indicates that the user is doing something wrong and it would be better if clap could tell the user that what he's doing doesn't make any sense.

In my specific scenario, I have a command which defaults to one of it subcommands, so `cmd --flag x` is the same as `cmd list --flag x`, while there's also other subcommands like `cmd foo --flag x`. One flag is shared between all subcommands (which I've tried making global), while some others are only available to `cmd` and `cmd list`. This would solve the issue of having an option that can be specified twice for that one shared flag, but I'd still need another solution preventing people from using the other `cmd list`-specific flags which can also be used with just `cmd` when another subcommand like `cmd foo` is used. So it's possible that this feature request is just a band-aid rather than solving the root issue.

### Describe the solution you'd like

Emit an error when specifying the same global argument multiple times.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @cd-work on 2022-07-15 16:11_

---

_Comment by @cd-work on 2022-07-15 17:51_

> I'd still need another solution preventing people from using the other cmd list-specific flags which can also be used with just cmd when another subcommand like cmd foo is used. So it's possible that this feature request is just a band-aid rather than solving the root issue.

I've found the `args_conflicts_with_subcommands` option, so I guess that solves my root issue in a more general way. Though the error is rather... unfortunate.

---

_Label `M-breaking-change` added by @epage on 2022-07-16 02:07_

---

_Comment by @epage on 2022-07-16 02:08_

Just reading the description,
- I would expect global arguments to be merged or overwritten based on their `Action` (granted, due to the current structure, we might not be able to handle this cleanly when custom actions are implemented).  
- Changing this would likely be considered a breaking change

---

_Label `A-parsing` added by @epage on 2022-11-08 04:30_

---

_Label `S-waiting-on-design` added by @epage on 2022-11-08 04:30_

---

_Referenced in [clap-rs/clap#1570](../../clap-rs/clap/issues/1570.md) on 2022-11-08 05:21_

---

_Referenced in [clap-rs/clap#5588](../../clap-rs/clap/issues/5588.md) on 2024-07-19 16:06_

---
