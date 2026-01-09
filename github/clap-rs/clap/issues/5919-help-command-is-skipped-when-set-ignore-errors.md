---
number: 5919
title: "`help` command is skipped when set `ignore_errors(true)` on the command."
type: issue
state: open
author: epage
labels:
  - C-bug
  - A-parsing
  - E-medium
assignees: []
created_at: 2025-02-19T17:48:59Z
updated_at: 2025-02-20T14:06:54Z
url: https://github.com/clap-rs/clap/issues/5919
synced_at: 2026-01-07T13:12:20-06:00
---

# `help` command is skipped when set `ignore_errors(true)` on the command.

---

_Issue opened by @epage on 2025-02-19 17:48_


### Discussed in https://github.com/clap-rs/clap/discussions/5917

<div type='discussions-op-text'>

<sup>Originally posted by **chungquantin** February 18, 2025</sup>
## Dependencies
```toml
clap = { version = "4.5", features = ["derive"] }
```

## Description

Hi, considering the below code: 

```diff
#[derive(Args)]
- #[command(args_conflicts_with_subcommands = true)]
+ #[command(args_conflicts_with_subcommands = true, ignore_errors = true)]
pub struct Cli {
	#[command(subcommand)]
	pub command: Command,
}

#[derive(Subcommand)]
pub enum Command {
	#[clap(alias = "p")]
	SubCommand(SubCommandA),
}
```

By adding the `ignore_errors`, the `help` command of the `SubCommandA` is no longer working. 

## How to reproduce an issue? 

Add `ignore_errors` to any `Command` that contains the `Subcommand` (does have `help`). </div>

CC @chungquantin

---

_Label `C-bug` added by @epage on 2025-02-19 17:49_

---

_Label `A-parsing` added by @epage on 2025-02-19 17:49_

---

_Label `E-medium` added by @epage on 2025-02-19 17:50_

---

_Comment by @imtherealnaska on 2025-02-20 13:42_

Hi, is anybody working on this issue ? I would like to pick this up 

---

_Comment by @epage on 2025-02-20 14:06_

Go for it!

---

_Referenced in [r0gue-io/pop-cli#420](../../r0gue-io/pop-cli/pulls/420.md) on 2025-02-26 07:42_

---

_Referenced in [clap-rs/clap#6052](../../clap-rs/clap/pulls/6052.md) on 2025-06-30 15:42_

---
