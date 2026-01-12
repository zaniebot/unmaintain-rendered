```yaml
number: 5358
title: "`args_conflicts_with_subcommands` is not overriding `subcommand_required = true`"
type: issue
state: open
author: foobl42
labels:
  - M-breaking-change
  - A-parsing
  - E-easy
assignees: []
created_at: 2024-02-16T19:12:08Z
updated_at: 2024-02-19T19:13:13Z
url: https://github.com/clap-rs/clap/issues/5358
synced_at: 2026-01-12T16:14:17Z
```

# `args_conflicts_with_subcommands` is not overriding `subcommand_required = true`

---

_@foobl42_

### Discussed in https://github.com/clap-rs/clap/discussions/5353

<div type='discussions-op-text'>

<sup>Originally posted by **fooblinator** February 15, 2024</sup>
I have a flag ("--about") at the "top level", meaning as a child argument of the program's command. It's a bool so is optional. I don't need it to be required. I also have a subcommand at the same level, which points to an enum of subcommands, whose variants point to other enums (nested subcommands).

As a config option, I have "args_conflicts_with_subcommands" set to true. Also, I have "arg_required_else_help" set to false, so I can see the generated error, when I fail to provide a command name. The error and usage makes good sense, and when not providing a subcommand, this message makes good suggestions about what to try:

> error: 'expenses' requires a subcommand but one was not provided
>   [subcommands: manage, track, help]
> 
> Usage: expenses [OPTIONS]
>        expenses  &lt;COMMAND&gt;
> 
> For more information, try '--help'.

The above happens when my subcommand is configured to be required (through *not* configuring it as an Option&lt;T&gt;).

Question is: is there a way to have the functionality where:

1. If you pass "--about" and NOT a subcommand, this is valid, and the program displays the about message.
2. If you pass a subcommand (and its' nested subcommands/args), this is valid, and the program will execute that subcommand functionality.
3. If you pass NOTHING, get the same error response you would get if the subcommand were required (like quoted above).

Here's some code I'm using:

```rust
[derive(Parser)]
#[command(about, version)]
#[command(
    arg_required_else_help = false,
    args_conflicts_with_subcommands = true,
)]
struct Cli {
    /// Print about
    #[arg(long, short)]
    about: bool,

    #[command(subcommand)]
    commands: CliCmds, // or `commands: Option<CliCmds>,` ???
}

#[derive(Subcommand)]
enum CliCmds {
    #[command(subcommand)]
    Manage(CliManageCmds),

    #[command(subcommand)]
    Track(CliTrackCmds),
}

/// Manage expense definitions
#[derive(Subcommand)]
#[command(arg_required_else_help = false)]
enum CliManageCmds {
    /// List existing expense definitions
    List,

    /// Search for existing expense definitions
    Search,

    /// Modify an existing expense definition
    Modify,

    /// Register a new expense definition
    Register,

    /// Archive an existing expense definition
    Archive,
}

/// Track defined expenses
#[derive(Subcommand, Debug)]
#[command(arg_required_else_help = false)]
enum CliTrackCmds {
    /// Mark an expense as staged to be paid
    Staged,

    /// Mark an expense as deferred (until a future pay period)
    Deferred,

    /// Mark an expense as paid (either fully or partially)
    Paid,
}
```

In summary, when I make my top-level commands required ("commands: CliCmds"), I can't use the "--about" top-level flag. But when I make those subcommands optional ("command: Option&lt;CliCmds&gt;"), then I can use the flag, but the helpful error doesn't display (when neither "--about" nor a subcommand is passed).</div>

---

_Label `M-breaking-change` added by @epage on 2024-02-19 19:10_

---

_Label `A-parsing` added by @epage on 2024-02-19 19:10_

---

_Renamed from "Optional flag at top-level conflicts with required top-level subcommands" to "`args_conflicts_with_subcommands` is not overriding `subcommand_required = true`" by @epage on 2024-02-19 19:11_

---

_Comment by @epage on 2024-02-19 19:13_

This is the other half of #3974  / #3940.  We considered that a breaking change and, out of a preponderance of caution, I'm taking that as a default stance here as well.

---

_Label `E-easy` added by @epage on 2024-02-19 19:13_

---
