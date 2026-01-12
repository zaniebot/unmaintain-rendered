```yaml
number: 5226
title: "`flatten_help` ignores `about` on subcommands and repeats top-level `about`"
type: issue
state: closed
author: Apanatshka
labels:
  - C-bug
assignees: []
created_at: 2023-11-27T13:30:08Z
updated_at: 2023-11-27T16:21:03Z
url: https://github.com/clap-rs/clap/issues/5226
synced_at: 2026-01-12T16:14:16Z
```

# `flatten_help` ignores `about` on subcommands and repeats top-level `about`

---

_@Apanatshka_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.1 (d5c2e9c34 2023-09-13)

### Clap Version

4.4.8

### Minimal reproducible code

```rust
use clap::{Parser, Subcommand};

#[derive(Subcommand, Debug)]
enum Command {
    #[command(about = "Hello!")]
    Public,
}

#[derive(Parser, Debug)]
#[command(about = "Our beautiful tool tagline", flatten_help = true)]
struct Opts {
    #[command(subcommand)]
    command: Command,
}

fn main() {
    let args = Opts::parse();
    println!("{:#?}", args);
}
```


### Steps to reproduce the bug with the above code

cargo run
cargo run -- help public

### Actual Behaviour

First command output:
```
Our beautiful tool tagline

Usage: clap-flatten-help public
       clap-flatten-help help [COMMAND]...

Options:
  -h, --help  Print help

clap-flatten-help public:
Our beautiful tool tagline
  -h, --help  Print help

clap-flatten-help help:
Our beautiful tool tagline
  [COMMAND]...  Print help for the subcommand(s)
```
Second command output:
```
Hello!

Usage: clap-flatten-help public

Options:
  -h, --help  Print help
```

### Expected Behaviour

First command output:
```
Our beautiful tool tagline

Usage: clap-flatten-help public
       clap-flatten-help help [COMMAND]...

Options:
  -h, --help  Print help

clap-flatten-help public:
Hello!
  -h, --help  Print help

clap-flatten-help help:
  [COMMAND]...  Print help for the subcommand(s)
```
Second command output:
```
Hello!

Usage: clap-flatten-help public

Options:
  -h, --help  Print help
```

### Additional Context

Note there are *two* differences in the expected behaviour:
1. The top-level about "Our beautiful tool tagline" should not be repeated in the `public` subcommand, I expect the about "Hello!" to show instead.
2. The top-level about should not be repeated in the `help` subcommand, that one shouldn't have any such line.

### Debug Output

<details><summary>First command output</summary>

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-flatten-help"
[clap_builder::builder::command]Command::_propagate:clap-flatten-help
[clap_builder::builder::command]Command::_check_help_and_version:clap-flatten-help expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-flatten-help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("help")
[clap_builder::parser::parser]Parser::parse_help_subcommand
[clap_builder::builder::command]Command::long_help_exists: false
[clap_builder::builder::command]Command::write_help_err: clap-flatten-help, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-flatten-help, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[clap_builder::builder::command]Command::_build: name="clap-flatten-help"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::builder::command]Command::_build: name="public"
[clap_builder::builder::command]Command::_propagate:public
[clap_builder::builder::command]Command::_check_help_and_version:public expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:public
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build: name="help"
[clap_builder::builder::command]Command::_propagate:help
[clap_builder::builder::command]Command::_check_help_and_version:help expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_propagate_global_args:help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:subcommand
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting usage_name of public to "clap-flatten-help public"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting bin_name of public to "clap-flatten-help public"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting display_name of public to "clap-flatten-help-public"
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting usage_name of help to "clap-flatten-help help"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting bin_name of help to "clap-flatten-help help"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting display_name of help to "clap-flatten-help-help"
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
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
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-flatten-help public
       clap-flatten-help help [COMMAND]...
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=6
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=14, spaces=10, avail=86
[clap_builder::builder::command]Command::_build: name="clap-flatten-help"
[clap_builder::builder::command]Command::_build: already built
[clap_builder::builder::command]Command::_build: name="public"
[clap_builder::builder::command]Command::_propagate:public
[clap_builder::builder::command]Command::_check_help_and_version:public expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:public
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build: name="help"
[clap_builder::builder::command]Command::_propagate:help
[clap_builder::builder::command]Command::_check_help_and_version:help expand_help_tree=true
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_propagate_global_args:help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:subcommand
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting usage_name of public to "clap-flatten-help public"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting bin_name of public to "clap-flatten-help public"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting display_name of public to "clap-flatten-help-public"
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_bin_names:iter: bin_name set...
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting usage_name of help to "clap-flatten-help help"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting bin_name of help to "clap-flatten-help help"
[clap_builder::builder::command]Command::_build_bin_names:iter: Setting display_name of help to "clap-flatten-help-help"
[clap_builder::builder::command]Command::_build_bin_names
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::output::help_template]HelpTemplate::write_flat_subcommands, cmd=clap-flatten-help, first=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args clap-flatten-help public
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=6
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=14, spaces=10, avail=86
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=subcommand
[clap_builder::output::help_template]HelpTemplate::write_args clap-flatten-help help
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=subcommand
[clap_builder::output::help_template]HelpTemplate::write_args: arg="subcommand" longest=12
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=subcommand
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=[COMMAND]...
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=[COMMAND]...
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=subcommand, next_line_help=false, longest=12
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=true arg_len=12, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=16, spaces=32, avail=84
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Our beautiful tool tagline

Usage: clap-flatten-help public
       clap-flatten-help help [COMMAND]...

Options:
  -h, --help  Print help

clap-flatten-help public:
Our beautiful tool tagline
  -h, --help  Print help

clap-flatten-help help:
Our beautiful tool tagline
  [COMMAND]...  Print help for the subcommand(s)
```
</details>
<details><summary>Second command output</summary>

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-flatten-help"
[clap_builder::builder::command]Command::_propagate:clap-flatten-help
[clap_builder::builder::command]Command::_check_help_and_version:clap-flatten-help expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_check_help_and_version: Building help subcommand
[clap_builder::builder::command]Command::_propagate_global_args:clap-flatten-help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=Some("help")
[clap_builder::parser::parser]Parser::parse_help_subcommand
[ clap_builder::output::usage]Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[ clap_builder::output::usage]Usage::get_required_usage_from: unrolled_reqs=[]
[ clap_builder::output::usage]Usage::get_required_usage_from: ret_val=[]
[clap_builder::builder::command]Command::_build_subcommand Setting bin_name of public to "clap-flatten-help public"
[clap_builder::builder::command]Command::_build_subcommand Setting display_name of public to "clap-flatten-help-public"
[clap_builder::builder::command]Command::_build: name="public"
[clap_builder::builder::command]Command::_propagate:public
[clap_builder::builder::command]Command::_check_help_and_version:public expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:public
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::builder::command]Command::long_help_exists: false
[clap_builder::builder::command]Command::write_help_err: clap-flatten-help-public, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=public, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
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
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-flatten-help public
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=6
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=6
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=14, spaces=10, avail=86
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Hello!

Usage: clap-flatten-help public

Options:
  -h, --help  Print help
```
</details>

---

_Label `C-bug` added by @Apanatshka on 2023-11-27 13:30_

---

_Referenced in [clap-rs/clap#5206](../../clap-rs/clap/pulls/5206.md) on 2023-11-27 13:32_

---

_Referenced in [clap-rs/clap#5227](../../clap-rs/clap/pulls/5227.md) on 2023-11-27 15:29_

---

_Closed by @epage on 2023-11-27 16:21_

---
