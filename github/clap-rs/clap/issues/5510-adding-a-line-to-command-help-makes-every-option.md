---
number: 5510
title: Adding a line to command help makes every option help multiline
type: issue
state: closed
author: stepancheg
labels:
  - C-bug
assignees: []
created_at: 2024-05-27T21:29:16Z
updated_at: 2024-06-05T01:00:52Z
url: https://github.com/clap-rs/clap/issues/5510
synced_at: 2026-01-07T13:12:20-06:00
---

# Adding a line to command help makes every option help multiline

---

_Issue opened by @stepancheg on 2024-05-27 21:29_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.78.0 (9b00956e5 2024-04-29)

### Clap Version

4.5.4

### Minimal reproducible code

```rust
use clap::Parser;

/// Help of command.
///
/// When this line is added, each option is printed multiline.
#[derive(clap::Parser)]
struct MyOpts {
    /// Help of foo.
    #[clap(long)]
    foo: String,
    /// Help of bar.
    #[clap(long)]
    bar: String,
}

fn main() {
    MyOpts::parse();
    println!("Hello, world!");
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- --help
```

### Actual Behaviour

In the example above, help is printed like this:

```
Help of command.

When this line is added, each option is printed multiline.

Usage: clap-bug-help --foo <FOO> --bar <BAR>

Options:
      --foo <FOO>
          Help of foo

      --bar <BAR>
          Help of bar

  -h, --help
          Print help (see a summary with '-h')
```

### Expected Behaviour

Remove the empty line and the last non-empty line from command help, and help looks like this:

```
Help of command

Usage: clap-bug-help --foo <FOO> --bar <BAR>

Options:
      --foo <FOO>  Help of foo
      --bar <BAR>  Help of bar
  -h, --help       Print help
```

- There's no obvious reason why changing command description affect individual options printing
- Also there's objectively no reason to print individual options help on multiple lines

### Additional Context

Full example: https://github.com/stepancheg/clap-bug-help/blob/master/src/main.rs

### Debug Output

Debug output with multiline command help:

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-bug-help"
[clap_builder::builder::command]Command::_propagate:clap-bug-help
[clap_builder::builder::command]Command::_check_help_and_version:clap-bug-help expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-bug-help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--help'
[clap_builder::parser::parser]Parser::parse_long_arg("help"): Presence validated
[clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Help: use_long=true
[clap_builder::builder::command]Command::long_help_exists: true
[clap_builder::builder::command]Command::write_help_err: clap-bug-help, use_long=true
[clap_builder::builder::command]Command::long_help_exists: true
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug-help, use_long=true
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=foo
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=bar
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["foo", "bar"]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-bug-help --foo <FOO> --bar <BAR>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=bar
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_args: arg="foo" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=bar
[clap_builder::output::help_template]HelpTemplate::write_args: arg="bar" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=foo, next_line_help=true, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=11, avail=90
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--bar <BAR>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=bar, next_line_help=true, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=11, avail=90
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=true, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=36, avail=90
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Help of command.

When this line is added, each option is printed multiline.

Usage: clap-bug-help --foo <FOO> --bar <BAR>

Options:
      --foo <FOO>
          Help of foo

      --bar <BAR>
          Help of bar

  -h, --help
          Print help (see a summary with '-h')
```

Debug output with single-line command help:

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="clap-bug-help"
[clap_builder::builder::command]Command::_propagate:clap-bug-help
[clap_builder::builder::command]Command::_check_help_and_version:clap-bug-help expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:clap-bug-help
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:foo
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:bar
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"--help"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("--help")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_long_arg
[clap_builder::parser::parser]Parser::parse_long_arg: Does it contain '='...
[clap_builder::parser::parser]Parser::parse_long_arg: Found valid arg or flag '--help'
[clap_builder::parser::parser]Parser::parse_long_arg("help"): Presence validated
[clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Long), source=CommandLine
[clap_builder::parser::parser]Help: use_long=true
[clap_builder::builder::command]Command::long_help_exists: false
[clap_builder::builder::command]Command::write_help_err: clap-bug-help, use_long=false
[clap_builder::builder::command]Command::long_help_exists: false
[  clap_builder::output::help]write_help
[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug-help, use_long=false
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_templated_help
[clap_builder::output::help_template]HelpTemplate::write_before_help
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=foo
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=bar
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is required
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["foo", "bar"]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-bug-help --foo <FOO> --bar <BAR>
[clap_builder::output::help_template]HelpTemplate::write_all_args
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args Options
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
[clap_builder::output::help_template]HelpTemplate::write_args: arg="foo" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
[clap_builder::output::help_template]HelpTemplate::write_args: arg="bar" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=11
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--bar <BAR>
[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=foo, next_line_help=false, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=11, avail=81
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--bar <BAR>
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=bar, next_line_help=false, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=11, avail=81
[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
[clap_builder::output::help_template]HelpTemplate::short
[clap_builder::output::help_template]HelpTemplate::long
[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=11
[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=7
[clap_builder::output::help_template]HelpTemplate::help
[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=10, avail=81
[clap_builder::output::help_template]HelpTemplate::write_after_help
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
Help of command

Usage: clap-bug-help --foo <FOO> --bar <BAR>

Options:
      --foo <FOO>  Help of foo
      --bar <BAR>  Help of bar
  -h, --help       Print help
```

Diff between these:

```
--- old	2024-05-27 22:20:04
+++ new	2024-05-27 22:19:59
@@ -1,5 +1,4 @@
-   Compiling clap-bug-help v0.1.0 (/Users/yozh/devel/clap-bug-help)
-    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.18s
+    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.01s
      Running `target/debug/clap-bug-help --help`
 [clap_builder::builder::command]Command::_do_parse
 [clap_builder::builder::command]Command::_build: name="clap-bug-help"
@@ -23,12 +22,12 @@
 [clap_builder::parser::parser]Parser::parse_long_arg("help"): Presence validated
 [clap_builder::parser::parser]Parser::react action=Help, identifier=Some(Long), source=CommandLine
 [clap_builder::parser::parser]Help: use_long=true
-[clap_builder::builder::command]Command::long_help_exists: true
-[clap_builder::builder::command]Command::write_help_err: clap-bug-help, use_long=true
-[clap_builder::builder::command]Command::long_help_exists: true
+[clap_builder::builder::command]Command::long_help_exists: false
+[clap_builder::builder::command]Command::write_help_err: clap-bug-help, use_long=false
+[clap_builder::builder::command]Command::long_help_exists: false
 [  clap_builder::output::help]write_help
-[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug-help, use_long=true
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
+[clap_builder::output::help_template]HelpTemplate::new cmd=clap-bug-help, use_long=false
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
 [clap_builder::output::help_template]HelpTemplate::write_templated_help
 [clap_builder::output::help_template]HelpTemplate::write_before_help
 [ clap_builder::output::usage]Usage::create_usage_no_title
@@ -48,42 +47,43 @@
 [ clap_builder::output::usage]Usage::write_subcommand_usage
 [ clap_builder::output::usage]Usage::create_usage_no_title: usage=clap-bug-help --foo <FOO> --bar <BAR>
 [clap_builder::output::help_template]HelpTemplate::write_all_args
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=bar
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
 [clap_builder::output::help_template]HelpTemplate::write_args Options
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
 [clap_builder::output::help_template]HelpTemplate::write_args: arg="foo" longest=11
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=bar
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
 [clap_builder::output::help_template]HelpTemplate::write_args: arg="bar" longest=11
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=help
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
 [clap_builder::output::help_template]HelpTemplate::write_args: arg="help" longest=11
-[clap_builder::output::help_template]should_show_arg: use_long=true, arg=foo
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=foo
 [clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=bar
+[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--bar <BAR>
+[clap_builder::output::help_template]should_show_arg: use_long=false, arg=help
+[clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
 [clap_builder::output::help_template]HelpTemplate::spec_vals: a=--foo <FOO>
 [clap_builder::output::help_template]HelpTemplate::short
 [clap_builder::output::help_template]HelpTemplate::long
-[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=foo, next_line_help=true, longest=11
-[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
+[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=foo, next_line_help=false, longest=11
+[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
 [clap_builder::output::help_template]HelpTemplate::help
-[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
-[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=11, avail=90
+[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=11, avail=81
 [clap_builder::output::help_template]HelpTemplate::spec_vals: a=--bar <BAR>
 [clap_builder::output::help_template]HelpTemplate::short
 [clap_builder::output::help_template]HelpTemplate::long
-[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=bar, next_line_help=true, longest=11
-[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
+[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=bar, next_line_help=false, longest=11
+[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=11, spaces=2
 [clap_builder::output::help_template]HelpTemplate::help
-[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
-[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=11, avail=90
+[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=11, avail=81
 [clap_builder::output::help_template]HelpTemplate::spec_vals: a=--help
 [clap_builder::output::help_template]HelpTemplate::short
 [clap_builder::output::help_template]HelpTemplate::long
-[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=true, longest=11
-[clap_builder::output::help_template]HelpTemplate::align_to_about: printing long help so skip alignment
+[clap_builder::output::help_template]HelpTemplate::align_to_about: arg=help, next_line_help=false, longest=11
+[clap_builder::output::help_template]HelpTemplate::align_to_about: positional=false arg_len=6, spaces=7
 [clap_builder::output::help_template]HelpTemplate::help
-[clap_builder::output::help_template]HelpTemplate::help: Next Line...true
-[clap_builder::output::help_template]HelpTemplate::help: help_width=10, spaces=36, avail=90
+[clap_builder::output::help_template]HelpTemplate::help: help_width=19, spaces=10, avail=81
 [clap_builder::output::help_template]HelpTemplate::write_after_help
 [clap_builder::builder::command]Command::color: Color setting...
 [clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @stepancheg on 2024-05-27 21:29_

---

_Comment by @epage on 2024-05-28 16:26_

When you use a multi-line doc-string, it maps to the `long` version of that attribute.  So in this case, you are setting `about` and `long_about`.  By specifying a `long_about`, you are opting in to `-h` being "short help" and `--help` being "long help".  You just happen to have no other long help and don't want the extra doc string output to be in your help output.

You have to ways to opt-out of long help while still maintaining that doc string.
- `#[command(long_about = None)]`
- Adding an explicit `-h` / `--help` flag and set [`#[arg(action = ArgAction::HelpShort)]`](https://docs.rs/clap/latest/clap/enum.ArgAction.html#variant.HelpShort)

As this is working as expected, I'm going to close this.  If there is a reason we should reconsider that, let us know!

---

_Closed by @epage on 2024-05-28 16:26_

---

_Comment by @stepancheg on 2024-06-05 01:00_

OK, then I think the issue is that long help should not split descriptions into multiple lines when all of them fit on one line.

Anyway, if this is by design, let it be.

Thank you very much for explanations!

---
