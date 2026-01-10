---
number: 3921
title: Support multiple values in native completions
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - A-completion
  - E-help-wanted
  - E-easy
  - ":money_with_wings: $20"
assignees: []
created_at: 2022-07-13T14:19:09Z
updated_at: 2024-08-08T16:48:49Z
url: https://github.com/clap-rs/clap/issues/3921
synced_at: 2026-01-10T01:27:49Z
---

# Support multiple values in native completions

---

_Issue opened by @epage on 2022-07-13 14:19_

Blocked on #3920

Both for flags and positional arguments

- [code](https://github.com/clap-rs/clap/blob/master/clap_complete/src/dynamic.rs)
- [tests](https://github.com/clap-rs/clap/blob/master/clap_complete/tests/dynamic.rs)
- [argcomplete](https://pypi.org/project/argcomplete/) and [cobra](https://github.com/spf13/cobra) can serve as examples

Tasks
- [x] #5601

---

_Label `C-enhancement` added by @epage on 2022-07-13 14:19_

---

_Label `A-completion` added by @epage on 2022-07-13 14:19_

---

_Label `E-easy` added by @epage on 2022-07-13 14:19_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2022-07-13 14:20_

---

_Label `:money_with_wings: $20` added by @epage on 2022-09-13 14:36_

---

_Label `E-help-wanted` added by @epage on 2022-09-20 14:29_

---

_Referenced in [clap-rs/clap#5539](../../clap-rs/clap/pulls/5539.md) on 2024-07-11 04:48_

---

_Referenced in [clap-rs/clap#5587](../../clap-rs/clap/issues/5587.md) on 2024-07-22 18:02_

---

_Comment by @shannmu on 2024-07-25 08:42_

I have added support to complete multiple values for flags.
But I have some confusion for positional arguments. How does `clap::Parser` parse the parsing of positional arguments, especially when there are multiple positional arguments with `num_args` setting? I have found the relevant code https://github.com/clap-rs/clap/blob/6b18d7725c6077a3da955d199617eb1fb3b88ea2/clap_builder/src/parser/parser.rs#L289-L404 it would be better if there were documentation explaining it.

---

_Comment by @epage on 2024-07-25 15:38_

Another relevant piece of code is
https://github.com/clap-rs/clap/blob/6b18d7725c6077a3da955d199617eb1fb3b88ea2/clap_builder/src/builder/debug_asserts.rs#L487-L674

Positionals must either be
- A fixed number of values (`num_args(5)`)
- `last` / `trailing_var_arg`
- second to last argument and `last` is set on the last

I might be missing some corner cases in there but that is also likely the order of importance for supporting this.  We can start with just `num_args(N)` and have separate issues for `last` and `trailing_var_arg` support.

---

_Comment by @shannmu on 2024-07-25 17:10_

```
use clap::{CommandFactory, Parser};
use clap_complete::dynamic::shells::CompleteCommand;
#[derive(Parser, Debug)]
#[clap(name = "dynamic", about = "A dynamic command line tool")]
struct Cli {
    /// The subcommand to run complete
    #[command(subcommand)]
    complete: Option<CompleteCommand>,
    #[clap(value_parser = ["pos_a"], num_args = 2, required = true, index = 1)]
    positional_a: Option<String>,
    #[clap(value_parser = ["pos_b"], num_args = 2, required = true, index = 2)]
    positional_b: Option<String>,
    #[clap(value_parser = ["pos_c"], num_args = 2, last = true, index = 3)]
    positional_c: Option<String>,
}
fn main() {
    let cli = Cli::parse();
    if let Some(completions) = cli.complete {
        completions.complete(&mut Cli::command());
    }
    // normal logic continues...
}
```
Command line:
```
dynamic pos_a pos_a pos_b pos_b -- pos_c pos_c
```
will report an error:
```
error: 2 values required for '<POSITIONAL_A> <POSITIONAL_A>' but 4 were provided

Usage: dynamic <POSITIONAL_A> <POSITIONAL_A> <POSITIONAL_B> <POSITIONAL_B> [-- <POSITIONAL_C> <POSITIONAL_C>]

For more information, try '--help'.
```
How should I use this cli? 

---

_Comment by @epage on 2024-07-25 17:25_

Huh, looks like something isn't working quite right.  I even tried throwing in `--divider` flags to see if that would cause the positionals to `resolve_pending` and advance the counter and it doesn't.

Updated code
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { path = "../clap", features = ["derive"] }
---

use clap::Parser;
use clap::builder::ArgAction;

#[derive(Parser, Debug)]
#[command(name = "dynamic", about = "A dynamic command line tool")]
struct Cli {
    #[arg(value_parser = ["pos_a"], num_args = 2, action = ArgAction::Set, required = true)]
    positional_a: Vec<String>,
    #[arg(value_parser = ["pos_b"], num_args = 2, action = ArgAction::Set, required = true)]
    positional_b: Vec<String>,
    #[arg(value_parser = ["pos_c"], num_args = 2, action = ArgAction::Set, last = true)]
    positional_c: Vec<String>,
    #[arg(short, long, action = ArgAction::Count)]
    divider: u8,
}
fn main() {
    let cli = Cli::parse();
    print!("{cli:?}");
}
```

---

_Comment by @shannmu on 2024-07-25 18:12_

Is it possible that positional arguments were not designed to be used this way (multiple positional arguments with `num_args` set)? If not, this might be a `Parser` related bug. Understanding this is pretty important for resolving the issue, as it affects when to complete the positional arguments.
I'll keep an eye on this issue. If there's a bug related to the `Parser`, I'd like to fix it.

---

_Comment by @epage on 2024-07-25 18:21_

Some thought was put into positionals and `num_args` but not in fully scaling it out like this.  These are bugs and would ideally be fixed but are not blockers for the work being done here.

---

_Referenced in [clap-rs/clap#5601](../../clap-rs/clap/pulls/5601.md) on 2024-07-26 13:05_

---

_Comment by @epage on 2024-08-08 16:48_

#5601 got us multi-value support.

It left out
- `trailing_var_arg`: noted as unblocking #3166
- `last`: noted as unblocking #3166
- `dynamic --flag=bar1 [TAB]`: this isn't support in clap's parser

By controlling the scope of this issue, it looks like we can close it out.

---

_Closed by @epage on 2024-08-08 16:48_

---
