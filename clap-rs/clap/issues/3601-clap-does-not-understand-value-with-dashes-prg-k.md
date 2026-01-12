```yaml
number: 3601
title: "clap does not understand value with dashes, \"prg -k -- -value\""
type: issue
state: closed
author: ayrapetovai
labels:
  - C-bug
assignees: []
created_at: 2022-04-01T17:05:35Z
updated_at: 2022-04-04T14:30:51Z
url: https://github.com/clap-rs/clap/issues/3601
synced_at: 2026-01-12T16:14:15Z
```

# clap does not understand value with dashes, "prg -k -- -value"

---

_@ayrapetovai_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.55.0-nightly (150fad30e 2021-06-19)

### Clap Version

3.1.8

### Minimal reproducible code

```rust
use clap::{Arg, command};

fn main() {
   let matches = command!()
        .arg(Arg::new("expression")
            .long("expression")
            .short('e')
            .takes_value(true)
        .get_matches();
}
```


### Steps to reproduce the bug with the above code

cargo run -- -e -1
cargo run -- -e -- '-1'

### Actual Behaviour

When run app with a key and passing an argument with -- and a '-' prefix ('prg -k -- -1'), clap thinks that the argument is a next key. And prints
> If you tried to supply `-1` as a value rather than a flag, use `-- -1`
but I do used `-- -1`!

### Expected Behaviour

clap is expected to think that '-- -1' is one argument "-1".

### Additional Context

$ binc -e -- '-1'
error: Found argument '-1' which wasn't expected, or isn't valid in this context

### Debug Output

```text
$ binc -e -- -1
[        clap::build::command] 	App::_do_parse
[        clap::build::command] 	App::_build
[        clap::build::command] 	App::_propagate:binc
[        clap::build::command] 	App::_check_help_and_version: binc
[        clap::build::command] 	App::_check_help_and_version: Removing `-h` from help
[        clap::build::command] 	App::_propagate_global_args:binc
[        clap::build::command] 	App::_derive_display_order:binc
[  clap::build::debug_asserts] 	Command::_debug_asserts
[  clap::build::debug_asserts] 	Arg::_debug_asserts:help
[  clap::build::debug_asserts] 	Arg::_debug_asserts:version
[  clap::build::debug_asserts] 	Arg::_debug_asserts:v
[  clap::build::debug_asserts] 	Arg::_debug_asserts:history
[  clap::build::debug_asserts] 	Arg::_debug_asserts:expression
[  clap::build::debug_asserts] 	Arg::_debug_asserts:format
[  clap::build::debug_asserts] 	Command::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-e")' ([45, 101])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-e")
[         clap::parse::parser] 	Parser::get_matches_with: sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: short_arg=RawOsStr("e")
[         clap::parse::parser] 	Parser::parse_short_arg:iter:e
[         clap::parse::parser] 	Parser::parse_short_arg:iter:e: cur_idx:=1
[         clap::parse::parser] 	Parser::parse_short_arg:iter:e: Found valid opt or flag
[         clap::parse::parser] 	Parser::parse_short_arg:iter:e: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=RawOsStr("e")
[         clap::parse::parser] 	Parser::parse_opt; opt=expression, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::remove_overrides: id=expression
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of_arg: id=expression
[        clap::build::command] 	App::groups_for_arg: id=expression
[        clap::build::command] 	App::groups_for_arg: id=expression
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(expression)
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("--")' ([45, 45])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: cur_idx:=2
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	Parser::get_matches_with: After parse_long_arg NoArg
[         clap::parse::parser] 	Parser::get_matches_with: setting TrailingVals=true
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing 'RawOsStr("-1")' ([45, 49])
[         clap::parse::parser] 	Parser::get_matches_with: Positional counter...1
[         clap::parse::parser] 	Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser] 	Parser::possible_subcommand: arg=RawOsStr("-1")
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_help_usage; incl_reqs=true
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage] 	Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage] 	Usage::needs_options_tag
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=help
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=version
[         clap::output::usage] 	Usage::needs_options_tag:iter Option is built-in
[         clap::output::usage] 	Usage::needs_options_tag:iter: f=v
[        clap::build::command] 	App::groups_for_arg: id=v
[         clap::output::usage] 	Usage::needs_options_tag:iter: [OPTIONS] required
[         clap::output::usage] 	Usage::create_help_usage: usage=binc [OPTIONS]
[        clap::build::command] 	App::color: Color setting...
[        clap::build::command] 	Auto
error: Found argument '-1' which wasn't expected, or isn't valid in this context

	If you tried to supply `-1` as a value rather than a flag, use `-- -1`

```

---

_Label `C-bug` added by @ayrapetovai on 2022-04-01 17:05_

---

_Comment by @epage on 2022-04-01 17:36_

Did you try passing `arg.allow_hyphen_values(true)`
https://docs.rs/clap/latest/clap/struct.Arg.html#method.allow_hyphen_values

---

_Comment by @ayrapetovai on 2022-04-02 07:37_

Thanks for fast reply.  
The applying of `Arg::allow_hyphen_values(true)`
```rust
Arg::new("expression")
  .long("expression")
  .short('e')
  .takes_value(true)
  .allow_hyphen_values(true)
```
leads to this result
```
$ binc -e -- -1
error: Found argument '-1' which wasn't expected, or isn't valid in this context

	If you tried to supply `-1` as a value rather than a flag, use `-- -1`

USAGE:
    binc [OPTIONS]

For more information try --help
```
The same result as without `Arg::allow_hyphen_values(true)`. This does not seem an expected behaviour to me. May be I am wrong...

Nevertheless, with `Arg::allow_hyphen_values(true)` applied the variant
```
$ binc -e -1
```
works. -1 is treated as value for the key -e

---

_Comment by @epage on 2022-04-04 14:30_

iirc `--` is meant to escape positional arguments, so it cannot be used for passing an argument to an option.  So it seems this is now working as expected and closing this out.

If there is any concern with the resolution, let us know what it is!

---

_Closed by @epage on 2022-04-04 14:30_

---
