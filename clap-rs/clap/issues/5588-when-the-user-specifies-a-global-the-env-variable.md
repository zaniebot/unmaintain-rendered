```yaml
number: 5588
title: When the user specifies a global, the env variable is still validated
type: issue
state: open
author: xobs
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2024-07-19T14:34:37Z
updated_at: 2024-07-19T16:06:59Z
url: https://github.com/clap-rs/clap/issues/5588
synced_at: 2026-01-12T16:14:17Z
```

# When the user specifies a global, the env variable is still validated

---

_@xobs_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.79.0 (129f3b996 2024-06-10)

### Clap Version

4.5.9

### Minimal reproducible code

```rust
use clap::{Args, Parser, Subcommand};

#[derive(Parser)]
struct Cli {
    #[clap(subcommand)]
    command: Command,

    #[clap(flatten)]
    global_args: GlobalArgs,
}

#[derive(Args, Debug)]
struct GlobalArgs {
    // This fails if you run `claptest -v info`,
    // but works if you delete either `global = true` or `env = "V"`
    #[clap(global = true, short, env = "V")]
    verbose: bool,
}

#[derive(Subcommand)]
enum Command {
    Info(InfoArgs),
}

#[derive(Args)]
pub struct InfoArgs {
}

fn main() {
    let args = std::env::args().collect::<Vec<String>>();

    if args.contains(&"-v".to_owned()) {
        std::env::set_var("V", "1");
    }

    let cli = Cli::parse_from(args);
    if cli.global_args.verbose {
        println!("Verbose mode enabled");
    } else {
        println!("Verbose mode disabled");
    }
}
```


### Steps to reproduce the bug with the above code

Cargo.toml:
```toml
[dependencies]
clap = { version = "4", features = ["derive", "env"] }
```

Run with:

```
cargo run -- -v info
```

Afterwards, remove `global = true` OR `env = "V"` and observe that it works.

### Actual Behaviour

```console
$ cargo run -- -v info
   Compiling claptest v0.1.0 (E:\Code\claptest)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.66s
     Running `target\debug\claptest.exe -v info`
error: invalid value '1' for '-v'
  [possible values: true, false]

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\claptest.exe -v info` (exit code: 2)

$
```

### Expected Behaviour

When removing `global = true` or `env = "V"` it works correctly:

```
$ cargo run -- -v info
   Compiling claptest v0.1.0 (E:\Code\claptest)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.68s
     Running `target\debug\claptest.exe -v info`
Verbose mode enabled
$
```

### Additional Context

The purpose of this code is to allow passing `V=1` to a function that sets up logging before clap is run. Since `env_logger` cannot change its level once it's initialized, we need to do this odd dance.

The issue here is that behaviour changes with `env = "V"` and `global = true`. It seems to only happen when the following three things are true:

1. The argument has `global = true` AND `env = "V"`
2. The environment variable is 1 OR `-v` is specified
3. A subcommand is present

If any condition is missing, the issue does not trigger.

### Debug Output

```
$ cargo run -- -v info
    Blocking waiting for file lock on package cache
   Compiling rustc-demangle v0.1.24
   Compiling cfg-if v1.0.0
   Compiling backtrace v0.3.73
   Compiling clap_builder v4.5.9
   Compiling clap v4.5.9
   Compiling claptest v0.1.0 (E:\Code\claptest)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.90s
     Running `target\debug\claptest.exe -v info`
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="claptest"
[clap_builder::builder::command]Command::_propagate:claptest
[clap_builder::builder::command]Command::_check_help_and_version:claptest expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:claptest
[clap_builder::builder::command]Command::_propagate pushing "verbose" to info
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:verbose
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-v"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-v")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "v", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['v']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:v
[clap_builder::parser::parser]Parser::parse_short_arg:iter:v: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Parser::react: has default_missing_vals
[clap_builder::parser::parser]Parser::remove_overrides: id="verbose"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="verbose", source=CommandLine
[clap_builder::builder::command]Command::groups_for_arg: id="verbose"
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="GlobalArgs", source=CommandLine
[clap_builder::parser::parser]Parser::push_arg_values: ["true"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg ValuesDone
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"info"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("info")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("info")
[clap_builder::parser::parser]Parser::parse_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of info to "claptest.exe info"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of info to "claptest-info"
[clap_builder::builder::command]Command::_build: name="info"
[clap_builder::builder::command]Command::_propagate:info
[clap_builder::builder::command]Command::_check_help_and_version:info expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:info
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:verbose
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::parse_subcommand: About to parse sc=info
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::add_env
[clap_builder::parser::parser]Parser::add_env: Checking arg `-v`
[clap_builder::parser::parser]Parser::add_env: Found an opt with value="1"
[clap_builder::parser::parser]Parser::react action=SetTrue, identifier=None, source=EnvVariable
[clap_builder::parser::arg_matcher]ArgMatcher::start_custom_arg: id="verbose", source=EnvVariable
[clap_builder::builder::command]Command::groups_for_arg: id="verbose"
[clap_builder::parser::parser]Parser::push_arg_values: ["1"]
[clap_builder::parser::parser]Parser::add_single_val_to_arg: cur_idx:=1
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: invalid value '1' for '-v'
  [possible values: true, false]

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\claptest.exe -v info` (exit code: 2)
$
```

---

_Label `C-bug` added by @xobs on 2024-07-19 14:34_

---

_Comment by @epage on 2024-07-19 14:53_

I don't think I quite understand your concern.

You are telling clap that `--verbose` should have a `V` environment variable associated with it.  Most of the other changes you mention seem tangential to that; the main problem seems to be about whether `1` is accepted for `--verbose`s `V`.  The default `value_parser` for `bool` is [`BoolValueParser`](https://docs.rs/clap/latest/clap/builder/struct.BoolValueParser.html).  You might be interested in [`value_parser = BoolishValueParser`](https://docs.rs/clap/latest/clap/builder/struct.BoolishValueParser.html).

---

_Comment by @xobs on 2024-07-19 15:02_

Isn't it the case where `bool` types will set the value to `true` if they exist? If so, it shouldn't matter whether the value is `1`, `true`, or something else.

The issue is that it does this normally, unless you have both `global = true` and `env = "V"`.

I think what happens is that with `global = true` and `env = "V"` it checks both the environment AND the flag, but with only one or the other it short-circuits if the `-v` argument is present.

The short-circuit is what was confusing me -- why would `global = true` cause it to evaluate both command line flags and environment variables, even if the command line flag exists? If that's a known behaviour, then that explains this issue and it can be closed.

---

_Renamed from "Cryptic error: `invalid value "1" for -v`" to "When the user specifies a global, the env variable is still validated" by @epage on 2024-07-19 16:05_

---

_Label `A-parsing` added by @epage on 2024-07-19 16:05_

---

_Label `S-waiting-on-design` added by @epage on 2024-07-19 16:05_

---

_Comment by @epage on 2024-07-19 16:06_

> The short-circuit is what was confusing me -

Thanks for the clarification

Currently, we parse a command at a time, including
- applying envs
- applying defaults
- validating

and then we process globals.

There are subtle aspects to this that will need care to resolve.  In particular, a solution should keep in mind some of our other issues around globals
- #1204 
- #1546 
- #1570
- #3938
- #5020

---
