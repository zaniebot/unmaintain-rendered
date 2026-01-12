```yaml
number: 3249
title: "short argument doesn't get parsed with allow_hyphen_values"
type: issue
state: closed
author: crowlKats
labels:
  - C-bug
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2022-01-03T21:59:36Z
updated_at: 2022-01-11T18:21:02Z
url: https://github.com/clap-rs/clap/issues/3249
synced_at: 2026-01-12T16:14:14Z
```

# short argument doesn't get parsed with allow_hyphen_values

---

_@crowlKats_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.57.0

### Clap Version

3.0.0

### Minimal reproducible code

```rust
fn main() {
    let app = clap::App::new("foo")
        .arg(
            clap::Arg::with_name("cmd")
                .required(true)
                .multiple(true)
                .allow_hyphen_values(true),
        )
        .arg(
            clap::Arg::with_name("name")
                .long("name")
                .short('n')
                .takes_value(true)
                .required(false),
        )
        .arg(
            clap::Arg::with_name("root")
                .long("root")
                .takes_value(true)
                .multiple(false),
        );

    let matches = app.get_matches();

    let cmd_values = matches.values_of("cmd").unwrap();
    let mut cmd = vec![];
    for value in cmd_values {
        cmd.push(value.to_string());
    }
    println!("{:?}", cmd);

    let name = matches.value_of("name").map(|s| s.to_string());
    println!("{:?}", name);

    let root = if matches.is_present("root") {
        let install_root = matches.value_of("root").unwrap();
        Some(std::path::PathBuf::from(install_root))
    } else {
        None
    };
    println!("{:?}", root);
}
```


### Steps to reproduce the bug with the above code

`cargo run -- --root ./foo -n test https://localhost:5545/echo.ts`

### Actual Behaviour

When i run the command, the output is:
```
Some("./foo")
None
["-n", "test", "https://localhost:5545/echo.ts"]
```

### Expected Behaviour

With clap 2.34.0 (with code adjuments to get it to compile, no actual changes to logic with exception of `.multiple_*` settings on arguments), i get this output:
```
Some("./foo")
Some("test")
["https://localhost:5545/echo.ts"]
```
which is what i would expect

### Additional Context

This is a blocker for Deno to upgrade to clap v3 (denoland/deno#13266)

### Debug Output

```
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:foo
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_propagate_global_args:foo
[            clap::build::app]  App::_derive_display_order:foo
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:cmd
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:name
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:root
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--root")' ([45, 45, 114, 111, 111, 116])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("--root")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_long_arg
[         clap::parse::parser]  Parser::parse_long_arg: cur_idx:=1
[         clap::parse::parser]  Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser]  No
[         clap::parse::parser]  Parser::parse_long_arg: Found valid opt or flag '--root <root>'
[         clap::parse::parser]  Parser::parse_long_arg: Found an opt with value 'None'
[         clap::parse::parser]  Parser::parse_opt; opt=root, val=None
[         clap::parse::parser]  Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL)
[         clap::parse::parser]  Parser::parse_opt; Checking for val...
[         clap::parse::parser]  Parser::parse_opt: More arg vals required...
[         clap::parse::parser]  Parser::remove_overrides: id=root
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=root
[            clap::build::app]  App::groups_for_arg: id=root
[            clap::build::app]  App::groups_for_arg: id=root
[         clap::parse::parser]  Parser::get_matches_with: After parse_long_arg Opt(root)
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("./foo")' ([46, 47, 102, 111, 111])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=root, val=RawOsStr("./foo")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."./foo"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app]  App::groups_for_arg: id=root
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=root
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("-n")' ([45, 110])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("-n")
[         clap::parse::parser]  Parser::get_matches_with: sc=None
[         clap::parse::parser]  Parser::parse_short_arg: short_arg=RawOsStr("n")
[         clap::parse::parser]  Parser::get_matches_with: After parse_short_arg MaybeHyphenValue
[         clap::parse::parser]  Parser::remove_overrides: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=cmd
[            clap::build::app]  App::groups_for_arg: id=cmd
[         clap::parse::parser]  Parser::add_val_to_arg; arg=cmd, val=RawOsStr("-n")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."-n"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=3
[            clap::build::app]  App::groups_for_arg: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=cmd
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("test")' ([116, 101, 115, 116])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=cmd
[            clap::build::app]  App::groups_for_arg: id=cmd
[         clap::parse::parser]  Parser::add_val_to_arg; arg=cmd, val=RawOsStr("test")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."test"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=4
[            clap::build::app]  App::groups_for_arg: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=cmd
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("https://localhost:5545/echo.ts")' ([104, 116, 116, 112, 115, 58, 47, 47, 108, 111, 99, 97, 108, 104, 111, 115, 116, 58, 53, 53, 52, 53, 47, 101, 99, 104, 111, 46, 116, 115])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::remove_overrides: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of_arg: id=cmd
[            clap::build::app]  App::groups_for_arg: id=cmd
[         clap::parse::parser]  Parser::add_val_to_arg; arg=cmd, val=RawOsStr("https://localhost:5545/echo.ts")
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_values=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."https://localhost:5545/echo.ts"
[         clap::parse::parser]  Parser::add_single_val_to_arg: cur_idx:=5
[            clap::build::app]  App::groups_for_arg: id=cmd
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=cmd
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:name:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:name: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:name: doesn't have default missing vals
[         clap::parse::parser]  Parser::add_defaults:iter:root:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:root: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:root: doesn't have default missing vals
[         clap::parse::parser]  Parser::add_defaults:iter:cmd:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:cmd: doesn't have default vals
[         clap::parse::parser]  Parser::add_value:iter:cmd: doesn't have default missing vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:root
[      clap::parse::validator]  Validator::validate_exclusive:iter:cmd
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=root
[      clap::parse::validator]  Conflicts::gather_conflicts
[            clap::build::app]  App::groups_for_arg: id=root
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=root, conflicts=[]
[            clap::build::app]  App::groups_for_arg: id=cmd
[      clap::parse::validator]  Conflicts::gather_direct_conflicts id=cmd, conflicts=[]
[      clap::parse::validator]  Validator::validate_conflicts::iter: id=cmd
[      clap::parse::validator]  Conflicts::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: cmd, children: [] }])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:root
[      clap::parse::validator]  Validator::gather_requirements:iter:cmd
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:root: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "./foo",
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
[      clap::parse::validator]  Validator::validate_arg_values: arg="root"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "root"=1
[      clap::parse::validator]  Validator::validate_matched_args:iter:cmd: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "-n",
                            "test",
                            "https://localhost:5545/echo.ts",
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
[      clap::parse::validator]  Validator::validate_arg_values: arg="cmd"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "cmd"=3
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[help]
```

---

_Label `C-bug` added by @crowlKats on 2022-01-03 21:59_

---

_Label `A-parsing` added by @epage on 2022-01-04 01:29_

---

_Label `S-triage` added by @epage on 2022-01-04 01:29_

---

_Comment by @epage on 2022-01-04 01:32_

I've modified the example so
- argument definitions and reading are done in a consistent order for easier comparing
- it is nearly compatible with clap2 (just need to change the `short`s type)

I'm surprised this worked in clap2 because of `allow_hyphen_values(true)` on the positional argument.  I'm a bit unsure what to say since it is doing what was asked.

---

_Comment by @crowlKats on 2022-01-04 06:14_

To make this more interesting: if you add `.last(true)` for `cmd`:
clap v3 will error with `Found argument '-n' which wasn't expected, or isn't valid in this context`, 
whereas in clap 2.34 it will error with `Found argument 'https://localhost:5545/echo.ts' which wasn't expected, or isn't valid in this context`

regardless of that, so would you say we should just remove `allow_hyphen_values(true)` and that would keep the behaviour of what we had with clap v2?

---

_Referenced in [clap-rs/clap#3250](../../clap-rs/clap/pulls/3250.md) on 2022-01-04 15:16_

---

_Comment by @epage on 2022-01-04 15:52_

v3.0.2 contains the fix for `.last(true)`.

If it helps, what `cargo run` does is:
```rust
        .setting(AppSettings::TrailingVarArg)
        .arg(Arg::with_name("args").multiple(true))
```
(granted, this is clap2, about to port to clap3)

---

_Referenced in [rust-lang/cargo#10253](../../rust-lang/cargo/pulls/10253.md) on 2022-01-04 18:15_

---

_Comment by @crowlKats on 2022-01-04 21:58_

I am a bit surprised that with `.last(true)` (in 3.0.2 & 2.34) it will say that `https://localhost:5545/echo.ts` is unexpected. What is the reasoning for this?
That aside, thanks, the change you suggested indeed does the job

---

_Comment by @epage on 2022-01-04 22:05_

If you are referring to with the command
```bash
$ cargo run -- --root ./foo -n test https://localhost:5545/echo.ts
```

The docs say

> This arg is the last, or final, positional argument (i.e. has the highest index) and is only able to be accessed via the -- syntax (i.e. $ prog args -- last_arg).

Keep in mind that this is related to `index` which is purely about positional arguments.  So while this can sound like it can help solve a problem here, it doesn't interact with named arguments at all.

`TrailingVarArg` is normally more of what people want, depending on the use case (like `cargo run`).

Its unclear to me what went into these design decisions.  They predate my involvement.

---

_Comment by @crowlKats on 2022-01-07 02:42_

Alright, thanks for all the information. Will close since the issue has been cleared up

---

_Closed by @crowlKats on 2022-01-07 02:42_

---

_Renamed from "short argument doesn't get parsed" to "short argument doesn't get parsed with allow_hyphen_values" by @epage on 2022-01-11 17:08_

---

_Label `S-triage` removed by @epage on 2022-01-11 18:21_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:21_

---
