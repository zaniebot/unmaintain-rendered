---
number: 4957
title: "#[clap(flatten, next_help_heading)] affects adjacent fields"
type: issue
state: closed
author: fsandhei
labels:
  - C-bug
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2023-06-08T07:13:44Z
updated_at: 2023-06-08T09:53:13Z
url: https://github.com/clap-rs/clap/issues/4957
synced_at: 2026-01-10T01:28:04Z
---

# #[clap(flatten, next_help_heading)] affects adjacent fields

---

_Issue opened by @fsandhei on 2023-06-08 07:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.69.0 (84c898d65 2023-04-16)

### Clap Version

clap = { version = "4.2.4", features = ["derive"] }

### Minimal reproducible code

```rust
use clap::{Args, Parser};

#[derive(Parser)]
struct Cli {
    #[arg(long = "version")]
    show_version: bool,
    #[clap(flatten, next_help_heading = "Miscellaneous options")]
    misc: MiscOpts,
    #[clap(long)]
    velocity: u16,
}

#[derive(Args)]
struct MiscOpts {
    #[arg(long)]
    pressure: u16,
    #[arg(long)]
    altitude: u16,
}

fn main() {
    let _cli = Cli::parse();
}
```


### Steps to reproduce the bug with the above code

```rust
cargo run -- -h
```

### Actual Behaviour

When using `next_help_heading` with `flatten` on a struct, it seems that the fields below that field will also be enveloped under the same help heading. This does not seem right.

![image](https://github.com/clap-rs/clap/assets/25849957/190b1eac-51e2-44d6-8b66-42ec11786f1a)


### Expected Behaviour

I would expect that clap would recognize that the fields that are declared under the flattened struct are the only fields that should be enveloped by that helper heading, not fields not related to it:

```rust
frsa@frsa-linux:~/dev/github/clap-flatten-test$ cargo run -- -h
   Compiling clap-flatten-test v0.1.0 (/home/frsa/dev/github/clap-flatten-test)
    Finished dev [unoptimized + debuginfo] target(s) in 0.76s
     Running `target/debug/clap-flatten-test -h`
Usage: clap-flatten-test --pressure <PRESSURE> --altitude <ALTITUDE> --velocity <VELOCITY>

Options:
      --version
  -h, --help     Print help

Miscellaneous options:
      --pressure <PRESSURE>
      --altitude <ALTITUDE>

      --velocity <VELOCITY>
```

I don't know how much this makes sense to do, but it does seem a bit misleading that fields not under `MiscOpts` in this context appears to be represented under "Miscellaneous options". This can be "circumvented" by declaring `velocity` before `misc`.

### Additional Context

_No response_

### Debug Output

```rust
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-flatten-test"
[clap_builder::builder::command]Command::_propagate:clap-flatten-test
[clap_builder::builder::command]Command::_check_help_and_version:clap-flatten-test expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-flatten-test
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:show_version
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:pressure
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:altitude
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:velocity
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-h"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-h")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "h", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['h']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:h
[clap_builder::parser::parser]Parser::parse_short_arg:iter:h: Found valid opt or flag
[clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Short), source=CommandLine
[clap_builder::parser::parser]Help: use_long=false
[clap_builder::builder::command]Command::write_help_err: clap-flatten-test, use_long=false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-flatten-test, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=show_version
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_help_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=show_version
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=pressure
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=altitude
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=velocity
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::get_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["pressure", "altitude", "velocity"]
[ clap_builder::output::usage]Usage::get_args: ret_val=[StyledStr("\u{1b}[1m--pressure\u{1b}[0m <PRESSURE>"), StyledStr("\u{1b}[1m--altitude\u{1b}[0m <ALTITUDE>"), StyledStr("\u{1b}[1m--velocity\u{1b}[0m <VELOCITY>")]
[ clap_builder::output::usage]Usage::create_help_usage: usage=clap-flatten-test --pressure <PRESSURE> --altitude <ALTITUDE> --velocity <VELOCITY>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=show_version
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=show_version
[clap_builder::output::help_template]HelpTemplate::write_args: arg="show_version" longest=9
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=9
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=show_version
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--version
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--version
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=show_version, next_line_help=false, longest=9
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=9, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=17, spaces=0, avail=83
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=9
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=5
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=17, spaces=10, avail=83
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=pressure
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=altitude
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=velocity
[clap_builder::output::help_template]HelpTemplate::write_args Miscellaneous options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=pressure
[clap_builder::output::help_template]HelpTemplate::write_args: arg="pressure" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=altitude
[clap_builder::output::help_template]HelpTemplate::write_args: arg="altitude" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=velocity
[clap_builder::output::help_template]HelpTemplate::write_args: arg="velocity" longest=21
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=pressure
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--pressure <PRESSURE>
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=altitude
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--altitude <ALTITUDE>
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=velocity
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--velocity <VELOCITY>
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--pressure <PRESSURE>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=pressure, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=21, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=0, avail=71
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--altitude <ALTITUDE>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=altitude, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=21, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=0, avail=71
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--velocity <VELOCITY>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=velocity, next_line_help=false, longest=21
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=21, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=29, spaces=0, avail=71
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Usage: clap-flatten-test --pressure <PRESSURE> --altitude <ALTITUDE> --velocity <VELOCITY>

Options:
      --version
  -h, --help     Print help

Miscellaneous options:
      --pressure <PRESSURE>
      --altitude <ALTITUDE>
      --velocity <VELOCITY>
```

---

_Label `C-bug` added by @fsandhei on 2023-06-08 07:13_

---

_Comment by @epage on 2023-06-08 09:53_

`next_help_heading` is a "sticky" value even so far as if you use it inside the flattened struct, it will leak out to the parent struct.  We used to prevent that latter case but pulled it out, see https://github.com/clap-rs/clap/pull/4222/commits/14c6ce0e838ef8d84c896254f02716e9e3c57c50.

Any change of this would be a breaking change and would have to wait until 5.0.

However, with us already having tried stuff in this direction and pulled it out, I'm inclined to say we shouldn't do this.  If there is a reason for us to re-evaluate this, let us know!

---

_Closed by @epage on 2023-06-08 09:53_

---

_Label `A-derive` added by @epage on 2023-06-08 09:53_

---

_Label `S-wont-fix` added by @epage on 2023-06-08 09:53_

---
