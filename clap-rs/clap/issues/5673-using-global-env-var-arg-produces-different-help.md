---
number: 5673
title: using global env var arg produces different help output
type: issue
state: closed
author: davepacheco
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2024-08-13T19:58:44Z
updated_at: 2024-08-14T13:27:18Z
url: https://github.com/clap-rs/clap/issues/5673
synced_at: 2026-01-10T01:28:15Z
---

# using global env var arg produces different help output

---

_Issue opened by @davepacheco on 2024-08-13 19:58_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.80.1 (3f5fd8dd4 2024-08-06)

### Clap Version

4.5.15

### Minimal reproducible code

```rust
use clap::Args;
use clap::Parser;
use clap::Subcommand;

#[derive(Debug, Clone, Parser)]
struct Cli {
    #[command(subcommand)]
    command: MyCommands,
}

#[derive(Debug, Clone, Subcommand)]
enum MyCommands {
    One(OneSubcommands),
}

#[derive(Debug, Clone, Args)]
struct OneSubcommands {
    #[clap(env = "MY_ARG", global = true)]
    my_arg: Option<String>,

    #[command(subcommand)]
    command: MySubcommand,
}

#[derive(Debug, Clone, Subcommand)]
enum MySubcommand {
    Two,
}

fn main() {
    let cli = Cli::parse();
    eprintln!("{:?}", cli);
}
```


### Steps to reproduce the bug with the above code

`MY_ARG= cargo run -- one`

### Actual Behaviour

It produces an explicit "error" message and _no_ additional help output.

```
$ MY_ARG= cargo run -- one
   Compiling clap-message v0.1.0 (/Users/dap/oxide/clap-bug)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.58s
     Running `target/debug/clap-message one`
error: 'clap-message one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-message one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

### Expected Behaviour

This is what happens if you don't provide `MY_ARG`:

```
$ cargo run -- one
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap-message one`
Usage: clap-message one [MY_ARG] <COMMAND>

Commands:
  two   
  help  Print this message or the help of the given subcommand(s)

Arguments:
  [MY_ARG]  [env: MY_ARG=]

Options:
  -h, --help  Print help
```

Here you get more detailed help output.

### Additional Context

If you make the environment variable non-global, you get the same behavior as if the env var wasn't specified.

If you don't provide an env var, but do provide the argument on the command line, you get the same behavior as if you'd provided the env var:

```
$ cargo run -- one my_value
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap-message one my_value`
error: 'clap-message one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-message one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

---

This behavior was particularly confusing because for the program where I found this, the env variable is an optional override that basically says which instance of a server you want the program to talk to.  It's something that people might set in their environment and forget about and would think it would have no impact on how the program reports usage messages that don't have anything to do with that env var.

### Debug Output

```
$ MY_ARG= cargo run -- one
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-message one`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-message"
[clap_builder::builder::command]Command::_propagate:clap-message
[clap_builder::builder::command]Command::_check_help_and_version:clap-message expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-message
[clap_builder::builder::command]Command::_propagate pushing "my_arg" to one
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:my_arg
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"one"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("one")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("one")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of one to "clap-message one"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of one to "clap-message-one"
[clap_builder::builder::command]Command::_build: name="one"
[clap_builder::builder::command]Command::_propagate:one
[clap_builder::builder::command]Command::_check_help_and_version:one expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:one
[clap_builder::builder::command]Command::_propagate pushing "my_arg" to two
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:my_arg
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=one
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Checking arg `[MY_ARG]`
[clap_builder::parser::parser]Parser::add_env: Found an opt with value=""
[clap_builder::parser::parser]Parser::react action=Set, identifier=None, source=EnvVariable
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="my_arg", source=EnvVariable
[clap_builder::builder::command]Command::groups_for_arg: id="my_arg"
[clap_builder::parser::parser]Parser::push_arg_values: [""]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: o=my_arg, pending=0
[clap_builder::parser::arg_matcher]ArgMatcher::needs_more_vals: expected=1, actual=0
[clap_builder::parser::parser]Parser::react not enough values passed in, leaving it to the validator to complain
[clap_builder::parser::parser]Parser::add_env: Checking arg `--help`
[clap_builder::parser::parser]Parser::add_defaults
[clap_builder::parser::parser]Parser::add_defaults:iter:my_arg:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:my_arg: doesn't have default vals
[clap_builder::parser::parser]Parser::add_defaults:iter:help:
[clap_builder::parser::parser]Parser::add_default_value: doesn't have conditional defaults
[clap_builder::parser::parser]Parser::add_default_value:iter:help: doesn't have default vals
[clap_builder::parser::validator]Validator::validate
[clap_builder::builder::command]Command::groups_for_arg: id="my_arg"
[clap_builder::parser::validator]Conflicts::gather_direct_conflicts id="my_arg", conflicts=[]
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: clap-message one [MY_ARG] <COMMAND>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: 'clap-message one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-message one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

---

_Label `C-bug` added by @davepacheco on 2024-08-13 19:58_

---

_Comment by @davepacheco on 2024-08-13 19:59_

The template wanted actual code and not a repo pointer, but in case it's useful, here's a repo that reproduces it:
https://github.com/davepacheco/clap-usage-message

---

_Label `A-parsing` added by @epage on 2024-08-13 20:14_

---

_Comment by @epage on 2024-08-13 20:18_

I'm able to reproduce the main behavior you mentioned
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive", "env"] }
---

use clap::Args;
use clap::Parser;
use clap::Subcommand;

#[derive(Debug, Clone, Parser)]
struct Cli {
    #[command(subcommand)]
    command: MyCommands,
}

#[derive(Debug, Clone, Subcommand)]
enum MyCommands {
    One(OneSubcommands),
}

#[derive(Debug, Clone, Args)]
struct OneSubcommands {
    #[clap(env = "MY_ARG", global = true)]
    my_arg: Option<String>,

    #[command(subcommand)]
    command: MySubcommand,
}

#[derive(Debug, Clone, Subcommand)]
enum MySubcommand {
    Two,
}

fn main() {
    let cli = Cli::parse();
    eprintln!("{:?}", cli);
}

```
```console
$ MY_ARG= ./clap-5673.rs one
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running `/home/epage/src/personal/cargo/target/debug/cargo -Zscript -Zmsrv-policy ./clap-5673.rs one`
warning: `package.edition` is unspecified, defaulting to `2021`
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.07s
     Running `/home/epage/.cargo/target/1b/a671870fa36384/debug/clap-5673 one`
error: 'clap-5673 one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-5673 one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

but I can't reproduce

> If you make the environment variable non-global, you get the same behavior as if the env var wasn't specified.

```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = { version = "4", features = ["derive", "env"] }
---

use clap::Args;
use clap::Parser;
use clap::Subcommand;

#[derive(Debug, Clone, Parser)]
struct Cli {
    #[command(subcommand)]
    command: MyCommands,
}

#[derive(Debug, Clone, Subcommand)]
enum MyCommands {
    One(OneSubcommands),
}

#[derive(Debug, Clone, Args)]
struct OneSubcommands {
    #[clap(env = "MY_ARG")]
    my_arg: Option<String>,

    #[command(subcommand)]
    command: MySubcommand,
}

#[derive(Debug, Clone, Subcommand)]
enum MySubcommand {
    Two,
}

fn main() {
    let cli = Cli::parse();
    eprintln!("{:?}", cli);
}
```
```console
$ MY_ARG= ./clap-5673.rs one
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.09s
     Running `/home/epage/src/personal/cargo/target/debug/cargo -Zscript -Zmsrv-policy ./clap-5673.rs one`
warning: `package.edition` is unspecified, defaulting to `2021`
   Compiling clap-5673 v0.0.0 (/home/epage/src/personal/dump)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.31s
     Running `/home/epage/.cargo/target/1b/a671870fa36384/debug/clap-5673 one`
error: 'clap-5673 one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-5673 one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

---

_Comment by @epage on 2024-08-13 20:23_

The help output comes from [`Command::arg_required_else_help`](https://docs.rs/clap/latest/clap/struct.Command.html#method.arg_required_else_help) is set by default for required subcommands.  It won't show up if any "explicit" values are specified, to avoid masking user errors.

Assuming `global` has nothing to to with this, this discrepancy you are seeing is because we consider an `env` to be explicitly specified value, like `std::env::args_os()` and unlike default values, so the help isn't shown and the rest of the validation occurs, showing that message.  This is why it matches the behavior of `cargo run -- one my_value`.

---

_Comment by @davepacheco on 2024-08-13 20:32_

Sorry, I had confused a few variations that I had tried.  If the argument is at the top level and global, as in:

```rust
use clap::Args;
use clap::Parser;
use clap::Subcommand;

#[derive(Debug, Clone, Parser)]
struct Cli {
    #[clap(env = "MY_ARG", global = true)]
    my_arg: Option<String>,

    #[command(subcommand)]
    command: MyCommands,
}

#[derive(Debug, Clone, Subcommand)]
enum MyCommands {
    One(OneSubcommands),
}

#[derive(Debug, Clone, Args)]
struct OneSubcommands {
    #[command(subcommand)]
    command: MySubcommand,
}

#[derive(Debug, Clone, Subcommand)]
enum MySubcommand {
    Two,
}

fn main() {
    let cli = Cli::parse();
    eprintln!("{:?}", cli);
}
```

Then I see:

```
dap@zathras clap-bug $ cargo run -- one 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-message one`
Usage: clap-message one [MY_ARG] <COMMAND>

Commands:
  two   
  help  Print this message or the help of the given subcommand(s)

Arguments:
  [MY_ARG]  [env: MY_ARG=]

Options:
  -h, --help  Print help
```

and:

```
$ MY_ARG= cargo run -- one 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap-message one`
error: 'clap-message one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-message one [MY_ARG] <COMMAND>

For more information, try '--help'.
```

If I then make the argument non-global, like:

```rust
use clap::Args;
use clap::Parser;
use clap::Subcommand;

#[derive(Debug, Clone, Parser)]
struct Cli {
    #[clap(env = "MY_ARG")]
    my_arg: Option<String>,

    #[command(subcommand)]
    command: MyCommands,
}

#[derive(Debug, Clone, Subcommand)]
enum MyCommands {
    One(OneSubcommands),
}

#[derive(Debug, Clone, Args)]
struct OneSubcommands {
    #[command(subcommand)]
    command: MySubcommand,
}

#[derive(Debug, Clone, Subcommand)]
enum MySubcommand {
    Two,
}

fn main() {
    let cli = Cli::parse();
    eprintln!("{:?}", cli);
}
```

then I get the same behavior (full help output) regardless of whether the variable was specified:

```
$ cargo run -- one 
   Compiling clap-message v0.1.0 (/Users/dap/oxide/clap-bug)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.95s
     Running `target/debug/clap-message one`
Usage: clap-message one <COMMAND>

Commands:
  two   
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help  Print help

$ MY_ARG= cargo run -- one 
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-message one`
Usage: clap-message one <COMMAND>

Commands:
  two   
  help  Print this message or the help of the given subcommand(s)

Options:
  -h, --help  Print help
```

> The help output comes from [`Command::arg_required_else_help`](https://docs.rs/clap/latest/clap/struct.Command.html#method.arg_required_else_help) is set by default for required subcommands. It won't show up if any "explicit" values are specified, to avoid masking user errors.
> 
> Assuming `global` has nothing to to with this, this discrepancy you are seeing is because we consider an `env` to be explicitly specified value, like `std::env::args_os()` and unlike default values, so the help isn't shown and the rest of the validation occurs, showing that message. This is why it matches the behavior of `cargo run -- one my_value`.

I think I can see the series of decisions that results in this behavior (though I'm not sure what user errors you're talking about masking if it didn't do this).  But I think it remains pretty surprising.

---

_Comment by @epage on 2024-08-13 20:53_

Looks like we have #3572 covering this case (had mislabeled it so I missed it in my search), closing in favor of that.

> I think I can see the series of decisions that results in this behavior (though I'm not sure what user errors you're talking about masking if it didn't do this). But I think it remains pretty surprising.

Take

```console
$ cargo run -- one my_value
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/clap-message one my_value`
error: 'clap-message one' requires a subcommand but one was not provided
  [subcommands: two, help]

Usage: clap-message one [MY_ARG] <COMMAND>

For more information, try '--help'.
```
If we showed help, we leave the user guessing as to what went wrong that caused the help to show up.  This is one reason why the old `SubcommandRequiredElseHelp` was dropped in favor of having just `arg_required_else_help`, in addition to simplifying clap overall.

which leaves us with whether the user explicitly setting an environment variable is more like `cargo run -- one my_value` or `cargo run -- one`

---

_Comment by @davepacheco on 2024-08-14 05:16_

Thanks for diving in!

> Looks like we have #3572 covering this case (had mislabeled it so I missed it in my search), closing in favor of that.

:+1: (but this one is currently still open, FYI)

> 
> > I think I can see the series of decisions that results in this behavior (though I'm not sure what user errors you're talking about masking if it didn't do this). But I think it remains pretty surprising.
> 
> Take
> 
> ```
> $ cargo run -- one my_value
>     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.03s
>      Running `target/debug/clap-message one my_value`
> error: 'clap-message one' requires a subcommand but one was not provided
>   [subcommands: two, help]
> 
> Usage: clap-message one [MY_ARG] <COMMAND>
> 
> For more information, try '--help'.
> ```
> 
> If we showed help, we leave the user guessing as to what went wrong that caused the help to show up.

Concretely, the help output would look like:

```
$ cargo run -- one my_value
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/clap-message one`
Usage: clap-message one [MY_ARG] <COMMAND>

Commands:
  two   
  help  Print this message or the help of the given subcommand(s)

Arguments:
  [MY_ARG]  [env: MY_ARG=]

Options:
  -h, --help  Print help
```

To me that's pretty clear: `my_value` pattern-matches with `[MY_ARG`] (even if I hadn't named it that way) and is obviously not one of the two commands listed.  The message `'clap-message one' requires a subcommand but one was not provided` doesn't seem particularly clearer -- that's pretty much what the `<COMMAND>` in the synopsis means, isn't it?  It might be clearer to convey that `my_value` is not one of the supported commands and was interpreted as a value for `MY_ARG`, but I'm not sure it's worth it.  Obviously, this is just one opinion.  But I gather I'm not the first person to find this behavior confusing -- I'm sorry I missed #5113 when I searched before filing this!

> This is one reason why the old `SubcommandRequiredElseHelp` was dropped in favor of having just `arg_required_else_help`, in addition to simplifying clap overall.

I didn't know about this one and just [looked it up](https://docs.rs/clap/latest/clap/struct.Command.html#method.arg_required_else_help).  The docs only say that applies to a command with no arguments present.  Are you saying it would apply here, too?  Is it the default behavior or something that could be used to work around this?

---

_Closed by @epage on 2024-08-14 13:25_

---

_Comment by @epage on 2024-08-14 13:27_

> > This is one reason why the old SubcommandRequiredElseHelp was dropped in favor of having just arg_required_else_help, in addition to simplifying clap overall.

> I didn't know about this one and just [looked it up](https://docs.rs/clap/latest/clap/struct.Command.html#method.arg_required_else_help). The docs only say that applies to a command with no arguments present. Are you saying it would apply here, too? Is it the default behavior or something that could be used to work around this?

`arg_required_else_help` is the behavior that is under discussion in this issue as it is what causes the help output to show up.  It is a default behavior for derive users when they have a required subcommand.

---

_Referenced in [oxidecomputer/omicron#6368](../../oxidecomputer/omicron/pulls/6368.md) on 2024-08-16 21:27_

---
