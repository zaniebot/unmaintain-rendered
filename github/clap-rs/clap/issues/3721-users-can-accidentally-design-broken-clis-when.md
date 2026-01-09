---
number: 3721
title: Users can accidentally design broken CLIs when combining subcommands with unbounded positional arguments
type: issue
state: open
author: NickCondron
labels:
  - C-bug
  - M-breaking-change
  - A-builder
  - E-easy
assignees: []
created_at: 2022-05-12T05:19:41Z
updated_at: 2022-05-12T09:00:47Z
url: https://github.com/clap-rs/clap/issues/3721
synced_at: 2026-01-07T13:12:19-06:00
---

# Users can accidentally design broken CLIs when combining subcommands with unbounded positional arguments

---

_Issue opened by @NickCondron on 2022-05-12 05:19_

Maintainer's notes:  Below is the original issue that came from misusing clap's API.  We could add a debug assert to help guide users to use the API correctly. 

---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.1.17

### Minimal reproducible code

```rust
use clap::{Args, Parser, Subcommand};
use std::{path};

#[derive(Debug, Parser)]
#[clap(subcommand_required=true)]
struct Cli {
    #[clap(subcommand)]
    command: Commands,

    /// files to search
    #[clap(global=true)]
    files: Vec<path::PathBuf>,
}

#[derive(Debug, Subcommand)]
enum Commands {
    /// Quickly filters replays based on metadata
    Filter(Filter),
    ///Search replay for something that happened
    Search(Search),
}

#[derive(Debug, Args)]
struct Filter {
    #[clap(short, long)]
    arg_filter: Option<String>,
}

#[derive(Debug, Args)]
struct Search {
    #[clap(short, long)]
    arg_search: Option<String>,
}

fn main() {
    let cli = Cli::parse();
}

```


### Steps to reproduce the bug with the above code

`$ cargo run -- -h`

### Actual Behaviour

```
USAGE:
    test-clap [FILES]... <SUBCOMMAND>

ARGS:
    <FILES>...    files to search
...
```

### Expected Behaviour

```
USAGE:
    test-clap <SUBCOMMAND> [Files]...

ARGS:
    <FILES>...    files to search
...
```
OR (perhaps it's up to taste)
```
USAGE:
    test-clap <SUBCOMMAND>

...
```

If subcommand is required then global arguments should only go after the subcommand is named.

### Additional Context

`$ cargo run -- help search` matches the expected behavior just fine

I also tried adding `args_conflicts_with_subcommands=true` to `Cli`, but it still doesn't produce expected behavior. It results in:
```
USAGE:
    slp-search [OPTIONS] [REPLAYS]...
    slp-search <SUBCOMMAND>
```
which seems wrong because the first usage case doesn't require a subcommand

I could add the `files` argument to both `Filter` and `Search`  structs, but this seems messy, and prevents me from having access to the shared argument before matching over the subcommands.

### Debug Output

_No response_

---

_Label `C-bug` added by @NickCondron on 2022-05-12 05:19_

---

_Comment by @epage on 2022-05-12 08:58_

> `#[clap(subcommand_required=true)]`

That is implicit when not using an `Option<Commands>`.  Or more precisely, `#[clap(subcommand_required_else_help=true)]` is though we will be changing that in https://github.com/clap-rs/clap/issues/3280 so its `#[clap(subcommand_required = true, arg_required_else_help = true)]`.

> If subcommand is required then global arguments should only go after the subcommand is named.

The usage is behaving as expected.  The command is described as having arguments that come before the subcommand, and so that is what is shown.

Now what might be considered a bug is that we allowed the combination of
- Unbounded number of positional arguments
- Subcommands
- `subcommand_precedence_over_arg = false`

We might want to consider adding a debug assert for that case.

> `$ cargo run -- help search` matches the expected behavior just fine

This is because we do not show optional arguments for the parent command in the subcommand help.  We only show the required ones for the direct parent and not for all parent subcommands.

> I also tried adding args_conflicts_with_subcommands=true to Cli, but it still doesn't produce expected behavior. 
> ...
> which seems wrong because the first usage case doesn't require a subcommand

Conflicts automatically override required arguments / subcommands.

> I could add the files argument to both Filter and Search structs, but this seems messy, and prevents me from having access to the shared argument before matching over the subcommands.

This is the correct solution.  `Commands` could provide an accessor method that does the dispatch through the enum for you.

---

_Label `E-easy` added by @epage on 2022-05-12 08:58_

---

_Renamed from "subcommand_required and global attributes cause bad usage" to "Users can accidentally design broken CLIs when combining subcommands with unbounded positional arguments" by @epage on 2022-05-12 08:59_

---

_Label `M-breaking-change` added by @epage on 2022-05-12 09:00_

---

_Label `A-builder` added by @epage on 2022-05-12 09:00_

---

_Referenced in [cucumber-rs/cucumber#215](../../cucumber-rs/cucumber/issues/215.md) on 2022-05-25 10:01_

---

_Referenced in [clap-rs/clap#3889](../../clap-rs/clap/issues/3889.md) on 2022-06-30 19:36_

---

_Referenced in [clap-rs/clap#4238](../../clap-rs/clap/issues/4238.md) on 2022-09-20 16:50_

---

_Referenced in [clap-rs/clap#6001](../../clap-rs/clap/issues/6001.md) on 2025-05-12 09:53_

---
