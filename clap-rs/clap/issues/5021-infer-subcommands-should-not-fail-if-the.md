```yaml
number: 5021
title: infer_subcommands should not fail if the ambiguity is over aliases
type: issue
state: closed
author: Xophmeister
labels:
  - C-bug
  - A-parsing
  - E-easy
assignees: []
created_at: 2023-07-19T14:19:14Z
updated_at: 2023-09-25T20:56:29Z
url: https://github.com/clap-rs/clap/issues/5021
synced_at: 2026-01-12T16:14:16Z
```

# infer_subcommands should not fail if the ambiguity is over aliases

---

_@Xophmeister_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.70.0 (90c541806 2023-05-31) (built from a source tarball)

### Clap Version

4.3.12

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Parser, Debug)]
#[command(infer_subcommands = true)]
struct TestCli {
    #[command(subcommand)]
    command: Commands,
}

#[derive(Debug, Subcommand)]
enum Commands {
    #[command(alias = "config")]
    Cfg,
}

fn main() {
    let cli = TestCli::parse();
    println!("{:?}", cli);
}
```

### Steps to reproduce the bug with the above code

```
cargo run -- c
```

### Actual Behaviour

It recognises the ambiguity between `config` and `cfg`:

```
error: unrecognized subcommand 'c'

  tip: some similar subcommands exist: 'config', 'cfg'
  tip: to pass 'c' as a value, use 'example -- c'

Usage: example <COMMAND>

For more information, try '--help'.
```

### Expected Behaviour

When an ambiguity only spans a single subcommand and its aliases, then it should resolve to that subcommand:

```
TestCli { command: Cfg }
```

### Additional Context

`infer_long_args` may also have the same behaviour; I haven't checked.

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="example"
[clap_builder::builder::command]Command::_propagate:example
[clap_builder::builder::command]Command::_check_help_and_version:example expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:example
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"c"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("c")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_help_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::get_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_args: ret_val=[]
[ clap_builder::output::usage]Usage::create_help_usage: usage=example <COMMAND>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @Xophmeister on 2023-07-19 14:19_

---

_Referenced in [clap-rs/clap#4815](../../clap-rs/clap/issues/4815.md) on 2023-07-19 14:51_

---

_Comment by @epage on 2023-07-19 14:53_

#4815 has some more information on this though that was covering multiple issues with inferring, so I've closed it out for one specific one, pointing here.

---

_Label `A-parsing` added by @epage on 2023-07-19 14:54_

---

_Comment by @SUPERCILEX on 2023-07-19 18:45_

Won't be able to do research anytime soon, but if it's decided that this issue should be fixed, I can put up my PR.

---

_Comment by @epage on 2023-07-19 19:26_

When there is ambiguity between a subcommand and its alias, it would be an error today.  Turning error cases into success is not a breaking change.

We have three different inference checks
- subcommand names: doesn't handle aliases
- subcommand longs: handles aliases
- longs: handles aliases

(based on code inspection and experimentation in #4815)

So resolving this would resolve an inconsistency.

Deciding on the consistency to error (prefer subcommand names approach) would likely be considered a breaking change.  It is also an overall lesser user experience.  The only reason we may consider it is if we felt it would offer enough binary size savings that we weigh that out over the impact of people affected by this behavior.   I have a hard time seeing how this would make a significant difference in binary size; many similar size changes go in without being measured.

So from that, I'd be willing to merge a fix for this.  I would ask that the fix ensure that tests exist for subcommand longs and longs, adding them if they don't.  A nice to have is to have the first commit add the test, verifying the existing behavior (error) and the second commit fixes the bug and updates the test.  This helps show confirm to reviewers (1) the test is testing the right thing and (2) the change has the intended afect.

---

_Label `E-easy` added by @epage on 2023-07-19 19:27_

---

_Closed by @epage on 2023-09-25 20:56_

---
