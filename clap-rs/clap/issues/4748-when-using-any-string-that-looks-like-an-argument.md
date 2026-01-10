---
number: 4748
title: when using any string that looks like an argument as a value to an argument, it will not parse the value, and instead attempt to load the non existent arg
type: issue
state: closed
author: lever1209
labels:
  - C-bug
assignees: []
created_at: 2023-03-07T04:17:35Z
updated_at: 2023-03-07T19:35:24Z
url: https://github.com/clap-rs/clap/issues/4748
synced_at: 2026-01-10T01:28:01Z
---

# when using any string that looks like an argument as a value to an argument, it will not parse the value, and instead attempt to load the non existent arg

---

_Issue opened by @lever1209 on 2023-03-07 04:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.68.0-nightly (3020239de 2023-01-09)

### Clap Version

4.1.8

### Minimal reproducible code

```rust
fn main() {
	let root_command = Command::new(env!("CARGO_PKG_NAME"))
		.arg(
			arg!(-a --arg <argument>)
				.value_parser(value_parser!(String))
				.action(ArgAction::Append),
		);

	let matches = root_command.get_matches();
	
	dbg!(matches);
}
```


### Steps to reproduce the bug with the above code

```
cargo run -- -a -c
```

### Actual Behaviour

```
error: unexpected argument '-c' found

Usage: wrapper-rs [OPTIONS]

For more information, try '--help'.
```

### Expected Behaviour

```
[src/main.rs:206] matches = ArgMatches {
    valid_args: [
        "arg",
        "help",
    ],
    valid_subcommands: [],
    args: FlatMap {
        keys: [
            "arg",
        ],
        values: [
            MatchedArg {
                source: Some(
                    CommandLine,
                ),
                indices: [
                    2,
                ],
                type_id: Some(
                    alloc::string::String,
                ),
                vals: [
                    [
                        AnyValue {
                            inner: alloc::string::String,
                        },
                    ],
                ],
                raw_vals: [
                    [
                        "-c",
                    ],
                ],
                ignore_case: false,
            },
        ],
    },
    subcommand: None,
}
```

### Additional Context

_No response_

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="wrapper-rs"
[      clap::builder::command]  Command::_propagate:wrapper-rs
[      clap::builder::command]  Command::_check_help_and_version:wrapper-rs expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --help
[      clap::builder::command]  Command::_propagate_global_args:wrapper-rs
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:arg
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:arg:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:arg: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
```

---

_Label `C-bug` added by @lever1209 on 2023-03-07 04:17_

---

_Comment by @epage on 2023-03-07 15:15_

This is expected behavior so we can report bugs related to invalid flags / missing values.

You can disable this with `Arg::allow_hyphen_values` or `Arg::allow_negative_numbers`.

---

_Comment by @lever1209 on 2023-03-07 19:35_

oh, thank you very much, i never noticed that

---

_Closed by @lever1209 on 2023-03-07 19:35_

---
