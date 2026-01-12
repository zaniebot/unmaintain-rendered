```yaml
number: 3859
title: "Defining `Arg` with `value_parser!` before positional args does not take value"
type: issue
state: closed
author: lopopolo
labels:
  - C-bug
assignees: []
created_at: 2022-06-21T06:49:56Z
updated_at: 2022-06-21T15:31:14Z
url: https://github.com/clap-rs/clap/issues/3859
synced_at: 2026-01-12T16:14:15Z
```

# Defining `Arg` with `value_parser!` before positional args does not take value

---

_@lopopolo_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.60.0 (7737e0b5c 2022-04-04)

### Clap Version

3.2.5

### Minimal reproducible code

## Good

```rust
use std::env;
use std::ffi::OsString;
use std::path::PathBuf;

use clap::builder::ArgAction;
use clap::{Arg, Command};

fn main() {
    let command = Command::new("artichoke")
        .about("Artichoke is a Ruby made with Rust.")
        .arg(
            Arg::new("commands")
                .short('e')
                .action(ArgAction::Append)
                .value_parser(clap::value_parser!(OsString))
                .help(r"one line of script. Several -e's allowed. Omit [programfile]"),
        )
        .arg(
            Arg::new("fixture")
                .long("with-fixture")
                .allow_invalid_utf8(true)
                .takes_value(true)
                .help("file whose contents will be read into the `$fixture` global"),
        )
        .arg(
            Arg::new("arguments")
                .multiple_values(true)
                .value_parser(clap::value_parser!(OsString)),
        )
        .version(env!("CARGO_PKG_VERSION"))
        .trailing_var_arg(true);

    let matches = command.try_get_matches_from(env::args_os()).unwrap();

    dbg!(matches
        .get_one::<OsString>("fixture")
        .cloned()
        .map(PathBuf::from));
}
```

## Bad

Apply this diff:

```diff
diff --git a/src/main.rs b/src/main.rs
index 9a58066..ce4d8c9 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -18,8 +18,7 @@ fn main() {
         .arg(
             Arg::new("fixture")
                 .long("with-fixture")
-                .allow_invalid_utf8(true)
-                .takes_value(true)
+                .value_parser(clap::value_parser!(OsString))
                 .help("file whose contents will be read into the `$fixture` global"),
         )
         .arg(
```

```rust
use std::env;
use std::ffi::OsString;
use std::path::PathBuf;

use clap::builder::ArgAction;
use clap::{Arg, Command};

fn main() {
    let command = Command::new("artichoke")
        .about("Artichoke is a Ruby made with Rust.")
        .arg(
            Arg::new("commands")
                .short('e')
                .action(ArgAction::Append)
                .value_parser(clap::value_parser!(OsString))
                .help(r"one line of script. Several -e's allowed. Omit [programfile]"),
        )
        .arg(
            Arg::new("fixture")
                .long("with-fixture")
                .value_parser(clap::value_parser!(OsString))
                .help("file whose contents will be read into the `$fixture` global"),
        )
        .arg(
            Arg::new("arguments")
                .multiple_values(true)
                .value_parser(clap::value_parser!(OsString)),
        )
        .version(env!("CARGO_PKG_VERSION"))
        .trailing_var_arg(true);

    let matches = command.try_get_matches_from(env::args_os()).unwrap();

    dbg!(matches
        .get_one::<OsString>("fixture")
        .cloned()
        .map(PathBuf::from));
}
```

### Steps to reproduce the bug with the above code

```console
$ cargo run -q -- --with-fixture c -e 'puts ARGV'
```

### Actual Behaviour

## Good

```
$ cargo run -q -- --with-fixture c -e 'puts ARGV'
[src/main.rs:35] matches.get_one::<OsString>("fixture").cloned().map(PathBuf::from) = Some(
    "c",
)
```

## Bad

```
$ cargo run -q -- --with-fixture c -e 'puts ARGV'
[src/main.rs:34] matches.get_one::<OsString>("fixture").cloned().map(PathBuf::from) = None
```

### Expected Behaviour

`clap::value_parser!` builder API should parse values for arguments that take values which appear before positional args.

### Additional Context

_No response_

### Debug Output

## Good

```console
$ cargo run -q -- --with-fixture c -e 'puts ARGV'
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="artichoke"
[      clap::builder::command]  Command::_propagate:artichoke
[      clap::builder::command]  Command::_check_help_and_version: artichoke
[      clap::builder::command]  Command::_propagate_global_args:artichoke
[      clap::builder::command]  Command::_derive_display_order:artichoke
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Arg::_debug_asserts:commands
[clap::builder::debug_asserts]  Arg::_debug_asserts:fixture
[clap::builder::debug_asserts]  Arg::_debug_asserts:arguments
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--with-fixture")' ([45, 45, 119, 105, 116, 104, 45, 102, 105, 120, 116, 117, 114, 101])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("--with-fixture")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_long_arg
[        clap::parser::parser]  Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser]  Parser::parse_long_arg: Found valid arg or flag '--with-fixture <fixture>'
[        clap::parser::parser]  Parser::parse_long_arg("with-fixture"): Found an arg with value 'None'
[        clap::parser::parser]  Parser::parse_opt_value; arg=fixture, val=None, has_eq=false
[        clap::parser::parser]  Parser::parse_opt_value; arg.settings=ArgFlags(TAKES_VAL | UTF8_NONE)
[        clap::parser::parser]  Parser::parse_opt_value; Checking for val...
[        clap::parser::parser]  Parser::parse_opt_value: More arg vals required...
[        clap::parser::parser]  Parser::get_matches_with: After parse_long_arg Opt(fixture)
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("c")' ([99])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::split_arg_values; arg=fixture, val=RawOsStr("c")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=fixture, resolved=0, pending=1
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-e")' ([45, 101])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("-e")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_short_arg: short_arg=ShortFlags { inner: RawOsStr("e"), utf8_prefix: CharIndices { front_offset: 0, iter: Chars(['e']) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_short_arg:iter:e
[        clap::parser::parser]  Parser::parse_short_arg:iter:e: Found valid opt or flag
[        clap::parser::parser]  Parser::parse_short_arg:iter:e: val=RawOsStr("") (bytes), val=[] (ascii), short_arg=ShortFlags { inner: RawOsStr("e"), utf8_prefix: CharIndices { front_offset: 1, iter: Chars([]) }, invalid_suffix: None }
[        clap::parser::parser]  Parser::parse_opt_value; arg=commands, val=None, has_eq=false
[        clap::parser::parser]  Parser::parse_opt_value; arg.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL)
[        clap::parser::parser]  Parser::parse_opt_value; Checking for val...
[        clap::parser::parser]  Parser::parse_opt_value: More arg vals required...
[        clap::parser::parser]  Parser::resolve_pending: id=fixture
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=Some(Long), source=CommandLine
[        clap::parser::parser]  Parser::react: cur_idx:=1
[        clap::parser::parser]  Parser::remove_overrides: id=fixture
[   clap::parser::arg_matcher]  ArgMatcher::start_occurrence_of_arg: id=fixture
[      clap::builder::command]  Command::groups_for_arg: id=fixture
[        clap::parser::parser]  Parser::push_arg_values: ["c"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command]  Command::groups_for_arg: id=fixture
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=fixture, resolved=1, pending=0
[        clap::parser::parser]  Parser::get_matches_with: After parse_short_arg Opt(commands)
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("puts ARGV")' ([112, 117, 116, 115, 32, 65, 82, 71, 86])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::split_arg_values; arg=commands, val=RawOsStr("puts ARGV")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=false, DontDelimTrailingVals=false
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=commands, resolved=0, pending=1
[        clap::parser::parser]  Parser::resolve_pending: id=commands
[        clap::parser::parser]  Parser::react action=Append, identifier=Some(Short), source=CommandLine
[        clap::parser::parser]  Parser::react: cur_idx:=3
[        clap::parser::parser]  Parser::remove_overrides: id=commands
[   clap::parser::arg_matcher]  ArgMatcher::start_custom_arg: id=commands, source=CommandLine
[      clap::builder::command]  Command::groups_for_arg: id=commands
[        clap::parser::parser]  Parser::push_arg_values: ["puts ARGV"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[      clap::builder::command]  Command::groups_for_arg: id=commands
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=commands, resolved=1, pending=0
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:version:
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:commands:
[        clap::parser::parser]  Parser::add_default_value:iter:commands: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:commands: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:fixture:
[        clap::parser::parser]  Parser::add_default_value:iter:fixture: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:fixture: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:arguments:
[        clap::parser::parser]  Parser::add_default_value:iter:arguments: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:arguments: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_exclusive:iter:fixture
[     clap::parser::validator]  Validator::validate_exclusive:iter:commands
[     clap::parser::validator]  Validator::validate_conflicts::iter: id=fixture
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=fixture
[      clap::builder::command]  Command::groups_for_arg: id=fixture
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=fixture, conflicts=[]
[      clap::builder::command]  Command::groups_for_arg: id=commands
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=commands, conflicts=[]
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_conflicts::iter: id=commands
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=commands
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::gather_requires:iter:fixture
[     clap::parser::validator]  Validator::gather_requires:iter:commands
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required_unless
[     clap::parser::validator]  Validator::validate_matched_args
[     clap::parser::validator]  Validator::validate_matched_args:iter:fixture: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: std::ffi::os_str::OsString,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator]  Validator::validate_arg_num_vals
[     clap::parser::validator]  Validator::validate_arg_values: arg="fixture"
[     clap::parser::validator]  Validator::validate_arg_num_occurs: "fixture"=1
[     clap::parser::validator]  Validator::validate_matched_args:iter:commands: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: std::ffi::os_str::OsString,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator]  Validator::validate_arg_num_vals
[     clap::parser::validator]  Validator::validate_arg_values: arg="commands"
[     clap::parser::validator]  Validator::validate_arg_num_occurs: "commands"=0
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help, version]
[src/main.rs:35] matches.get_one::<OsString>("fixture").cloned().map(PathBuf::from) = Some(
    "c",
)
```

## Bad

```console
$ cargo run -q -- --with-fixture c -e 'puts ARGV'
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="artichoke"
[      clap::builder::command]  Command::_propagate:artichoke
[      clap::builder::command]  Command::_check_help_and_version: artichoke
[      clap::builder::command]  Command::_propagate_global_args:artichoke
[      clap::builder::command]  Command::_derive_display_order:artichoke
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:help
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Arg::_debug_asserts:commands
[clap::builder::debug_asserts]  Arg::_debug_asserts:fixture
[clap::builder::debug_asserts]  Arg::_debug_asserts:arguments
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--with-fixture")' ([45, 45, 119, 105, 116, 104, 45, 102, 105, 120, 116, 117, 114, 101])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("--with-fixture")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_long_arg
[        clap::parser::parser]  Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser]  Parser::parse_long_arg: Found valid arg or flag '--with-fixture'
[        clap::parser::parser]  Parser::parse_long_arg("with-fixture"): Presence validated
[        clap::parser::parser]  Parser::react action=IncOccurrence, identifier=Some(Long), source=CommandLine
[        clap::parser::parser]  Parser::react: cur_idx:=1
[        clap::parser::parser]  Parser::remove_overrides: id=fixture
[   clap::parser::arg_matcher]  ArgMatcher::start_occurrence_of_arg: id=fixture
[      clap::builder::command]  Command::groups_for_arg: id=fixture
[        clap::parser::parser]  Parser::get_matches_with: After parse_long_arg ValuesDone
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("c")' ([99])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("c")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::split_arg_values; arg=arguments, val=RawOsStr("c")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=true, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-e")' ([45, 101])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::split_arg_values; arg=arguments, val=RawOsStr("-e")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=true, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("puts ARGV")' ([112, 117, 116, 115, 32, 65, 82, 71, 86])
[        clap::parser::parser]  Parser::get_matches_with: Positional counter...1
[        clap::parser::parser]  Parser::get_matches_with: Low index multiples...false
[        clap::parser::parser]  Parser::split_arg_values; arg=arguments, val=RawOsStr("puts ARGV")
[        clap::parser::parser]  Parser::split_arg_values; trailing_values=true, DontDelimTrailingVals=false
[        clap::parser::parser]  Parser::resolve_pending: id=arguments
[        clap::parser::parser]  Parser::react action=StoreValue, identifier=Some(Index), source=CommandLine
[        clap::parser::parser]  Parser::remove_overrides: id=arguments
[   clap::parser::arg_matcher]  ArgMatcher::start_occurrence_of_arg: id=arguments
[      clap::builder::command]  Command::groups_for_arg: id=arguments
[        clap::parser::parser]  Parser::push_arg_values: ["c", "-e", "puts ARGV"]
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[      clap::builder::command]  Command::groups_for_arg: id=arguments
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[      clap::builder::command]  Command::groups_for_arg: id=arguments
[        clap::parser::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[      clap::builder::command]  Command::groups_for_arg: id=arguments
[   clap::parser::arg_matcher]  ArgMatcher::needs_more_vals: o=arguments, resolved=3, pending=0
[        clap::parser::parser]  Parser::react not enough values passed in, leaving it to the validator to complain
[        clap::parser::parser]  Parser::add_defaults
[        clap::parser::parser]  Parser::add_defaults:iter:help:
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:help: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:version:
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:version: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:commands:
[        clap::parser::parser]  Parser::add_default_value:iter:commands: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:commands: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:fixture:
[        clap::parser::parser]  Parser::add_default_value:iter:fixture: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:fixture: doesn't have default vals
[        clap::parser::parser]  Parser::add_defaults:iter:arguments:
[        clap::parser::parser]  Parser::add_default_value:iter:arguments: doesn't have default missing vals
[        clap::parser::parser]  Parser::add_default_value: doesn't have conditional defaults
[        clap::parser::parser]  Parser::add_default_value:iter:arguments: doesn't have default vals
[     clap::parser::validator]  Validator::validate
[     clap::parser::validator]  Validator::validate_conflicts
[     clap::parser::validator]  Validator::validate_exclusive
[     clap::parser::validator]  Validator::validate_exclusive:iter:fixture
[     clap::parser::validator]  Validator::validate_exclusive:iter:arguments
[     clap::parser::validator]  Validator::validate_conflicts::iter: id=fixture
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=fixture
[      clap::builder::command]  Command::groups_for_arg: id=fixture
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=fixture, conflicts=[]
[      clap::builder::command]  Command::groups_for_arg: id=arguments
[     clap::parser::validator]  Conflicts::gather_direct_conflicts id=arguments, conflicts=[]
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_conflicts::iter: id=arguments
[     clap::parser::validator]  Conflicts::gather_conflicts: arg=arguments
[     clap::parser::validator]  Conflicts::gather_conflicts: conflicts=[]
[     clap::parser::validator]  Validator::validate_required: required=ChildGraph([])
[     clap::parser::validator]  Validator::gather_requires
[     clap::parser::validator]  Validator::gather_requires:iter:fixture
[     clap::parser::validator]  Validator::gather_requires:iter:arguments
[     clap::parser::validator]  Validator::validate_required: is_exclusive_present=false
[     clap::parser::validator]  Validator::validate_required_unless
[     clap::parser::validator]  Validator::validate_matched_args
[     clap::parser::validator]  Validator::validate_matched_args:iter:fixture: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator]  Validator::validate_arg_num_vals
[     clap::parser::validator]  Validator::validate_arg_values: arg="fixture"
[     clap::parser::validator]  Validator::validate_arg_num_occurs: "fixture"=1
[     clap::parser::validator]  Validator::validate_matched_args:iter:arguments: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            AnyValue {
                                inner: std::ffi::os_str::OsString,
                            },
                            AnyValue {
                                inner: std::ffi::os_str::OsString,
                            },
                            AnyValue {
                                inner: std::ffi::os_str::OsString,
                            },
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[     clap::parser::validator]  Validator::validate_arg_num_vals
[     clap::parser::validator]  Validator::validate_arg_values: arg="arguments"
[     clap::parser::validator]  Validator::validate_arg_num_occurs: "arguments"=3
[   clap::parser::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help, version]
[src/main.rs:34] matches.get_one::<OsString>("fixture").cloned().map(PathBuf::from) = None
```

---

_Label `C-bug` added by @lopopolo on 2022-06-21 06:49_

---

_Comment by @epage on 2022-06-21 13:58_

```diff
diff --git a/src/main.rs b/src/main.rs
index 9a58066..ce4d8c9 100644
--- a/src/main.rs
+++ b/src/main.rs
@@ -18,8 +18,7 @@ fn main() {
         .arg(
             Arg::new("fixture")
                 .long("with-fixture")
-                .allow_invalid_utf8(true)
-                .takes_value(true)
+                .value_parser(clap::value_parser!(OsString))
                 .help("file whose contents will be read into the `$fixture` global"),
         )
         .arg(
```
This patch is incorrect; `takes_value(true)` is still needed.  `value_parser` affects the parsing of env variables even for flags which have `takes_value(false)`, so `value_parser` does not imply `takes_value(true)`.

---

_Comment by @lopopolo on 2022-06-21 14:44_

Ahh good to know. Sorry for the bad report.

---

_Closed by @epage on 2022-06-21 14:46_

---

_Referenced in [artichoke/artichoke#1911](../../artichoke/artichoke/pulls/1911.md) on 2022-06-21 15:29_

---

_Comment by @lopopolo on 2022-06-21 15:31_

Thanks for setting me on the right path @epage.

I managed to get the 3.2.x style APIs working and I'm pretty happy with the result. I ran into a couple of panics while I got the turbofishes correct, but I think the code reads much nicer now.

---
