```yaml
number: 4341
title: "Hide `--help` (but still handle it) if `help` subcommand exists"
type: issue
state: open
author: joshtriplett
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2022-10-03T21:38:07Z
updated_at: 2022-10-03T21:42:32Z
url: https://github.com/clap-rs/clap/issues/4341
synced_at: 2026-01-12T16:14:15Z
```

# Hide `--help` (but still handle it) if `help` subcommand exists

---

_@joshtriplett_

For many CLIs with subcommands, a subcommand may be *required* (e.g. showing help if there's no subcommand). In this case, there will commonly be an automatic `help` subcommand, but *also* a `--help` option. For some CLIs this may be the *only* option.

```console
$ bit
Usage: foo <COMMAND>

Commands:
  build  
  sync  
  help   Print this message or the help of the given subcommand(s)

Options:
  -h, --help  Print help information
```

To improve usability and reduce the number of things for the user to read through, could clap default to hiding `--help` if there's a `help` subcommand, especially if a subcommand is *required*?

```console
$ bit
Usage: foo <COMMAND>

Commands:
  build  
  sync  
  help   Print this message or the help of the given subcommand(s)
```

---

_Label `A-help` added by @epage on 2022-10-03 21:38_

---

_Label `S-waiting-on-design` added by @epage on 2022-10-03 21:38_

---

_Label `C-enhancement` added by @epage on 2022-10-03 21:38_

---

_Renamed from "Usability improvement: hide `--help` (but still handle it) if `help` subcommand exists" to "Hide `--help` (but still handle it) if `help` subcommand exists" by @epage on 2022-10-03 21:38_

---

_Comment by @epage on 2022-10-03 21:41_

A simple approach would be `help_arg.hide_short_help(has_subcommands && !cmd.disable_help_subcommand)`

The issue also brings up checking `cmd.is_subcommand_required_set`.  Another relevant setting is `cmd.is_args_conflicts_with_subcommands_set()`

---

_Comment by @epage on 2022-10-03 21:42_

My main concerns
- Is this heuristic worth people having to undo it when it doesn't fit?
- Are the rules understandable for when things don't work as someone expects?
- How do we document this?

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-10-03 21:42_

---
