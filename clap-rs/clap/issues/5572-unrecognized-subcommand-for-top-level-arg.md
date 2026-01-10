---
number: 5572
title: Unrecognized subcommand for top level arg
type: issue
state: closed
author: nricciardi
labels:
  - C-bug
assignees: []
created_at: 2024-07-08T13:43:36Z
updated_at: 2024-07-09T23:14:36Z
url: https://github.com/clap-rs/clap/issues/5572
synced_at: 2026-01-10T01:28:13Z
---

# Unrecognized subcommand for top level arg

---

_Issue opened by @nricciardi on 2024-07-08 13:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.79.0 (129f3b996 2024-06-10)

### Clap Version

4.4.18

### Minimal reproducible code

```rust
let cli: Command = Command::new("nmd")
                .about("Official compiler to parse NMD")
                .version(VERSION)
                .subcommand_required(true)
                .arg_required_else_help(true)
                .arg(
                    Arg::new("verbose")
                        .short('v')
                        .long("verbose")
                        .help("set verbose mode")
                        .action(ArgAction::Set)
                        .default_value("info")
                )
                .subcommand(...)


let matches = cli.get_matches();

if let Some(verbose) = matches.get_one::<String>("verbose") {            
    
    let log_level = LevelFilter::from_str(verbose)?;

    Self::set_logger(log_level);
}
```


### Steps to reproduce the bug with the above code

```
cargo run -v debug [subcommand]
```
or
```
cargo run --verbose debug [subcommand]
```


### Actual Behaviour

```
error: unrecognized subcommand 'debug'

Usage: nmd [OPTIONS] <COMMAND>

For more information, try '--help'.
```

### Expected Behaviour

It should set log level as `debug`

### Additional Context

I'm developing CLI for my Markdown compiler (https://github.com/nricciardi/nmd). I have some `subcommand` in which I would set log level, so I insert `arg()` at the top in `Command::new()`

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="nmd"
[clap_builder::builder::command]Command::_propagate:nmd
[clap_builder::builder::command]Command::_check_help_and_version:nmd expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building default --version
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:nmd
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:verbose
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:version
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"debug"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("debug")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::get_matches_with: Positional counter...1
[clap_builder::parser::parser]Parser::get_matches_with: Low index multiples...false
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=verbose
[clap_builder::builder::command]Command::groups_for_arg: id="verbose"
[ clap_builder::output::usage]Usage::needs_options_tag:iter: [OPTIONS] required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: nmd [OPTIONS] <COMMAND>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
error: unrecognized subcommand 'debug'

Usage: nmd [OPTIONS] <COMMAND>

For more information, try '--help'.
```

---

_Label `C-bug` added by @nricciardi on 2024-07-08 13:43_

---

_Comment by @epage on 2024-07-08 14:58_

I think there might be something unclear about the reproduction steps.

I created the following code:
```rust
#!/usr/bin/env nargo
---
[dependencies]
clap = "4"
---

use clap::Command;
use clap::Arg;
use clap::builder::ArgAction;

fn main() {
    let cli: Command = Command::new("nmd")
                    .about("Official compiler to parse NMD")
                    .version("1.0")
                    .subcommand_required(true)
                    .arg_required_else_help(true)
                    .arg(
                        Arg::new("verbose")
                            .short('v')
                            .long("verbose")
                            .help("set verbose mode")
                            .action(ArgAction::Set)
                            .default_value("info")
                    )
                    .subcommand(Command::new("foo"));


    let matches = cli.get_matches();

    if let Some(verbose) = matches.get_one::<String>("verbose") {
        dbg!(verbose);
    }
    dbg!(matches.subcommand_name());
}
```
```console
$ ./clap-5572.rs --verbose debug foo
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.09s
     Running `/home/epage/src/personal/cargo/target/debug/cargo -Zscript ./clap-5572.rs --verbose debug foo`
warning: `package.edition` is unspecified, defaulting to `2021`
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.06s
     Running `/home/epage/.cargo/target/f7/135059ae5b9ba8/debug/clap-5572 --verbose debug foo`
[/home/epage/.cargo/target/f7/135059ae5b9ba8/clap-5572.rs:31:9] verbose = "debug"
[/home/epage/.cargo/target/f7/135059ae5b9ba8/clap-5572.rs:33:5] matches.subcommand_name() = Some(
    "foo",
)
```

---

_Comment by @nricciardi on 2024-07-08 15:48_

Sincerely, I don't know how it is possible. Maybe because you used clap v4 instead of v4.4.18?

On your machine, does my cli work? Anyway, I will try your code on mine.

---

_Comment by @nricciardi on 2024-07-09 23:14_

I tried (`clap = "4.4.18"`) your code, but it doesn't work. I understand the problem: I use `cargo` to run `nmd` code (my code). `cargo run` already has `-v --verbose` option.

In conclusion, the solution is `--`:

```
cargo run -- -v debug [subcommand]
```
or
```
cargo run -- --verbose debug [subcommand]
```



---

_Closed by @nricciardi on 2024-07-09 23:14_

---
