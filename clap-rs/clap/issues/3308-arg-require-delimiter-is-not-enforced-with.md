```yaml
number: 3308
title: "`Arg::require_delimiter` is not enforced with positional arguments"
type: issue
state: closed
author: cyqsimon
labels:
  - C-bug
assignees: []
created_at: 2022-01-18T11:48:54Z
updated_at: 2022-01-19T15:16:38Z
url: https://github.com/clap-rs/clap/issues/3308
synced_at: 2026-01-12T16:14:14Z
```

# `Arg::require_delimiter` is not enforced with positional arguments

---

_@cyqsimon_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.58.0

### Clap Version

3.0.9

### Minimal reproducible code

```rust
use clap::Parser;

#[derive(Debug, Parser)]
struct CliArgs {
    #[clap(required = true, value_delimiter = ',', require_delimiter = true)]
    list: Vec<u32>,
}

fn main() {
    let CliArgs { list } = CliArgs::parse();
    println!("List: {:?}", list);
}
```

### Steps to reproduce the bug with the above code

`cargo run -- 1 2 3 4 5`

### Actual Behaviour

`List: [1, 2, 3, 4, 5]`

### Expected Behaviour

Parse should fail with `ErrorKind::UnknownArgument`, as shown in [the example](https://docs.rs/clap/latest/clap/struct.Arg.html#method.require_delimiter).

### Additional Context

OS is Manjaro and shell is Bash

### Debug Output

```
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:rust_test
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_propagate_global_args:rust_test
[            clap::build::app]  App::_derive_display_order:rust_test
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:list
[clap::build::app::debug_asserts]       App::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("1")' ([49])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("1")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::remove_overrides: id=list
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=list
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_val_to_arg; arg=list, val=RawOsStr("1")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."1"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=1
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("2")' ([50])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=list
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=list
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_val_to_arg; arg=list, val=RawOsStr("2")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."2"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("3")' ([51])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=list
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=list
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_val_to_arg; arg=list, val=RawOsStr("3")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."3"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("4")' ([52])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=list
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=list
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_val_to_arg; arg=list, val=RawOsStr("4")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."4"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("5")' ([53])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=list
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=list
[            clap::build::app]  App::groups_for_arg: id=list
[         clap::parse::parser]  Parser::add_val_to_arg; arg=list, val=RawOsStr("5")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."5"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=5
[            clap::build::app]  App::groups_for_arg: id=list
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:list:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:list: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:list: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:list
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=list
[      clap::parse::validator]  Conflicts::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: list, children: [] }])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:list
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:list: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "1",
                            "2",
                            "3",
                            "4",
                            "5",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="list"
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "list"=5
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help]
```

---

_Label `C-bug` added by @cyqsimon on 2022-01-18 11:48_

---

_Comment by @epage on 2022-01-18 16:41_

clap 3's derive uses `multiple_occurrences` for `Vec`.  You need to opt-out of that to get the behavior you are wanting
```rust
use clap::Parser;

#[derive(Debug, Parser)]
struct CliArgs {
    #[clap(
        required = true,
        multiple_occurrences = false,
        value_delimiter = ',',
        require_delimiter = true
    )]
    list: Vec<u32>,
}

fn main() {
    let CliArgs { list } = CliArgs::parse();
    println!("List: {:?}", list);
}
```

See #1772 for more context.

I'm going to go ahead and close this, assuming this resolves the issue.  If not, feel free to let us know!

---

_Closed by @epage on 2022-01-18 16:41_

---

_Comment by @cyqsimon on 2022-01-19 08:54_

Hmm. This does indeed give me the intended behaviour; thank you for that. However I'm noticing that `multiple_occurences` and `require_delimiter` seems to essentially provide the same functionality (semantically speaking), albeit with opposite polarity. And if I'm understanding correctly, these two options are effective for different data types?

Anyways, this situation is just quite confusing from a user perspective IMO. I would imagine cleaning this up would cause quite a lot of API breakages and isn't ideal. So perhaps we can consider making some notes on this behaviour in the docs in the meanwhile?

---

_Comment by @epage on 2022-01-19 15:16_

The distinction between occurrences and values is more clear with flags.  `--foo 1 2 --bar 3` is an occurrence with 2 values followed by an occurrence with 1 value.

We support both multiple occurrences and values for positional arguments because they can be used in slightly different ways.  We also chose to go with occurrences for the `derive` so we'd be consistent in how we treat `Vec`, making it more predictable.

You can have multiple occurrences **and** require a delimiter.  `1,2 3` would be a positional version of the earlier example with using delimiters.

> Anyways, this situation is just quite confusing from a user perspective IMO. I would imagine cleaning this up would cause quite a lot of API breakages and isn't ideal. 

I am unsure what "clean up" there would be at this point.  If you have ideas, feel free to share them.

> So perhaps we can consider making some notes on this behaviour in the docs in the meanwhile?

What in particular are you thinking should be documented and where?  In the derive reference we [call out the meaning of `Vec<T>`](https://github.com/clap-rs/clap/tree/master/examples/derive_ref#arg-types).  #3314 has further increased the discoverability of the derive reference.  Past that, its also understanding the underlying builder API.

---
