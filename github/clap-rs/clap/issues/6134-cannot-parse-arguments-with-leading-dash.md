---
number: 6134
title: Cannot parse arguments with leading dash
type: issue
state: closed
author: just-ero
labels:
  - C-bug
assignees: []
created_at: 2025-09-19T09:00:08Z
updated_at: 2025-09-19T15:30:16Z
url: https://github.com/clap-rs/clap/issues/6134
synced_at: 2026-01-07T13:12:20-06:00
---

# Cannot parse arguments with leading dash

---

_Issue opened by @just-ero on 2025-09-19 09:00_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.86.0 (05f9846f8 2025-03-31)

### Clap Version

4.5.41

### Minimal reproducible code

```rust
#[derive(clap::Parser)]
#[command()]
struct Args {
    #[arg()]
    my_arg: String,
}

fn main() {
    _ = Args::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- "-foo"`

### Actual Behaviour

```
error: unexpected argument '-f' found

  tip: to pass '-f' as a value, use '-- -f'
```

### Expected Behaviour

No error.

### Additional Context

_No response_

### Debug Output

```
[clap_builder::builder::command]Command::_do_parse
[clap_builder::builder::command]Command::_build: name="matrix-to-gitlab"
[clap_builder::builder::command]Command::_propagate:matrix-to-gitlab
[clap_builder::builder::command]Command::_check_help_and_version:matrix-to-gitlab expand_help_tree=false
[clap_builder::builder::command]Command::long_help_exists
[clap_builder::builder::command]Command::_check_help_and_version: Building default --help
[clap_builder::builder::command]Command::_propagate_global_args:matrix-to-gitlab
[clap_builder::builder::debug_asserts]Command::_debug_asserts
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:my_arg
[clap_builder::builder::debug_asserts]Arg::_debug_asserts:help
[clap_builder::builder::debug_asserts]Command::_verify_positionals
[clap_builder::parser::parser]Parser::get_matches_with
[clap_builder::parser::parser]Parser::parse
[clap_builder::parser::parser]Parser::get_matches_with: Begin parsing '"-t9ErJApPG9RqRZ2.H-t74iaYe3XrGCttKszQ-ULvmQ-99LGBdLLCHYrh!YdRHFD"'
[clap_builder::parser::parser]Parser::possible_subcommand: arg=Ok("-t9ErJApPG9RqRZ2.H-t74iaYe3XrGCttKszQ-ULvmQ-99LGBdLLCHYrh!YdRHFD")
[clap_builder::parser::parser]Parser::get_matches_with: sc=None
[clap_builder::parser::parser]Parser::parse_short_arg: short_arg=ShortFlags { inner: "t9ErJApPG9RqRZ2.H-t74iaYe3XrGCttKszQ-ULvmQ-99LGBdLLCHYrh!YdRHFD", utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['t', '9', 'E', 'r', 'J', 'A', 'p', 'P', 'G', '9', 'R', 'q', 'R', 'Z', '2', '.', 'H', '-', 't', '7', '4', 'i', 'a', 'Y', 'e', '3', 'X', 'r', 'G', 'C', 't', 't', 'K', 's', 'z', 'Q', '-', 'U', 'L', 'v', 'm', 'Q', '-', '9', '9', 'L', 'G', 'B', 'd', 'L', 'L', 'C', 'H', 'Y', 'r', 'h', '!', 'Y', 'd', 'R', 'H', 'F', 'D']) }, invalid_suffix: None }
[clap_builder::parser::parser]Parser::parse_short_arg:iter:t
[clap_builder::parser::parser]Parser::get_matches_with: After parse_short_arg NoMatchingArg { arg: "-t" }
[ clap_builder::output::usage]Usage::create_usage_with_title
[ clap_builder::output::usage]Usage::create_usage_no_title
[ clap_builder::output::usage]Usage::write_help_usage
[ clap_builder::output::usage]Usage::write_arg_usage; incl_reqs=true
[ clap_builder::output::usage]Usage::needs_options_tag
[ clap_builder::output::usage]Usage::needs_options_tag:iter: f=help
[ clap_builder::output::usage]Usage::needs_options_tag:iter Option is built-in
[ clap_builder::output::usage]Usage::needs_options_tag: [OPTIONS] not required
[ clap_builder::output::usage]Usage::write_args: incls=[]
[ clap_builder::output::usage]Usage::get_args: unrolled_reqs=["my_arg"]
[ clap_builder::output::usage]Usage::write_subcommand_usage
[ clap_builder::output::usage]Usage::create_usage_with_title: usage=Usage: matrix-to-gitlab.exe <MY_ARG>
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
[clap_builder::builder::command]Command::color: Color setting...
[clap_builder::builder::command]Auto
```

---

_Label `C-bug` added by @just-ero on 2025-09-19 09:00_

---

_Comment by @vjgaur on 2025-09-19 11:33_

I am taking this up 

---

_Comment by @epage on 2025-09-19 11:35_

Have you tried enabling [`allow_hyphen_values = true`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.allow_hyphen_values)?

---

_Comment by @vjgaur on 2025-09-19 11:44_

> Have you tried enabling [`allow_hyphen_values = true`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.allow_hyphen_values)?

Are you asking me or the original issue creator ?
For me its first time going through clap repo, I was investigating the issue in parser.rs in parser_short_arg function 
Let me know if this is not the issue and is an expected behaviour.  If configuring allow_hyphen_values = true works than  this issue should be closed .

---

_Comment by @vjgaur on 2025-09-19 12:19_

@epage I have added the test cases in the app_setting.rs to verify this. Its working fine. shall I raise the PR

---

_Referenced in [clap-rs/clap#6135](../../clap-rs/clap/pulls/6135.md) on 2025-09-19 12:42_

---

_Comment by @vjgaur on 2025-09-19 12:43_

@epage @just-ero I have created the pr for this issue #[6134](https://github.com/clap-rs/clap/pull/6135) 

---

_Comment by @epage on 2025-09-19 15:30_

As this is working as expected, I'm going to go ahead and close

---

_Closed by @epage on 2025-09-19 15:30_

---

_Referenced in [clap-rs/clap#6137](../../clap-rs/clap/pulls/6137.md) on 2025-09-19 20:51_

---
