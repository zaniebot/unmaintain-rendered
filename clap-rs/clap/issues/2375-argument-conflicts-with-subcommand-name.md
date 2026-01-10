---
number: 2375
title: Argument conflicts with subcommand name
type: issue
state: open
author: x3rAx
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2021-03-02T11:04:52Z
updated_at: 2024-06-07T06:53:41Z
url: https://github.com/clap-rs/clap/issues/2375
synced_at: 2026-01-10T01:27:16Z
---

# Argument conflicts with subcommand name

---

_Issue opened by @x3rAx on 2021-03-02 11:04_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.50.0 (cb75ad5db 2021-02-10)

### Clap Version

clap = "3.0.0-beta.2"

### Minimal reproducible code

```rust
use std::path::PathBuf;
use clap::Clap;

fn main() {
    let _opts = Opts::parse();
}

/// This is a test
#[derive(Clap)]
struct Opts {
    #[clap(subcommand)]
    subcmd: SubCommand,
}

#[derive(Clap)]
enum SubCommand {
    File(File),
}

/// Work with a file
#[derive(Clap)]
struct File {
    /// An argument before another sub command
    file: PathBuf,
    #[clap(subcommand)]
    subcmd: TestSubCommand,
}

#[derive(Clap)]
enum TestSubCommand {
    Test(Test),
}

/// Work with a file
#[derive(Clap)]
struct Test {
    /// An argument of the `test` subcommand
    value: i32
}
```


### Steps to reproduce the bug with the above code

- `cargo run -- file test test 1`
- `cargo run -- file xtest test 1`

### Actual Behaviour

The first argument `file` is a subcommand, that accepts a filename and a sub-subcommand `test` which accepts a number.
If the filename is the same as the sub-subcommand, it fails with the following error:

```txt
error: Found argument '1' which wasn't expected, or isn't valid in this context

If you tried to supply `1` as a PATTERN use `-- 1`

USAGE:
    clap-test file <file> test <value>

For more information try --help
```

Putting  `--` anywhere between the arguments does not fix it but produces other errors.

If the filename is similar to the subcommand name, then it fails and proposes the "correct" subcommand:

```txt
error: The subcommand 'xtest' wasn't recognized

        Did you mean 'test'?

If you believe you received this message in error, try re-running with 'clap-test file -- xtest'

USAGE:
    clap-test file <file> <SUBCOMMAND>

For more information try --help
```

Again putting  `--` anywhere between the arguments does not fix it but produces other errors.

### Expected Behaviour

The argument after `file` should be treated as an argument (as it is not optional), not as a subcommand, just as the help text suggests:

```txt
USAGE:
    clap-test file <file> test <value>
```

### Additional Context

_No response_

### Debug Output

```txt
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:clap-test
[            clap::build::app]  App::_propagate:file
[            clap::build::app]  App::_propagate:test
[            clap::build::app]  App::_derive_display_order:clap-test
[            clap::build::app]  App::_derive_display_order:file
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_create_help_and_version: Building help
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"file"' ([102, 105, 108, 101])
[         clap::parse::parser]  Parser::is_new_arg: "file":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: probably value
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser]  Parser::possible_subcommand: arg="file"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=true, sc=Some("file")
[         clap::parse::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[]
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[            clap::build::app]  App::_propagate:clap-test
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:file
[            clap::build::app]  App::_propagate:test
[            clap::build::app]  App::_derive_display_order:file
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_create_help_and_version: Building help
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:file
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::parse_subcommand: About to parse sc=file
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"file"' ([102, 105, 108, 101])
[         clap::parse::parser]  Parser::is_new_arg: "file":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: probably value
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser]  Parser::possible_subcommand: arg="file"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=file, val="file"
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."file"
[            clap::build::app]  App::groups_for_arg: id=file
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=file
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=file
[            clap::build::app]  App::groups_for_arg: id=file
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"test"' ([116, 101, 115, 116])
[         clap::parse::parser]  Parser::is_new_arg: "test":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: probably value
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser]  Parser::possible_subcommand: arg="test"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=true, sc=Some("test")
[         clap::parse::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs=[file]
[         clap::output::usage]  Usage::get_required_usage_from:iter:file
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=["<file>"]
[            clap::build::app]  App::_propagate:file
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:test
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[clap::build::app::debug_asserts]       App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:value
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::parse_subcommand: About to parse sc=test
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"1"' ([49])
[         clap::parse::parser]  Parser::is_new_arg: "1":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: probably value
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser]  Parser::possible_subcommand: arg="1"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::add_val_to_arg; arg=value, val="1"
[         clap::parse::parser]  Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
[         clap::parse::parser]  Parser::add_single_val_to_arg: adding val..."1"
[            clap::build::app]  App::groups_for_arg: id=value
[    clap::parse::arg_matcher]  ArgMatcher::needs_more_vals: o=value
[    clap::parse::arg_matcher]  ArgMatcher::inc_occurrence_of: arg=value
[            clap::build::app]  App::groups_for_arg: id=value
[         clap::parse::parser]  Parser::remove_overrides
[         clap::parse::parser]  Parser::remove_overrides:iter:value
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:value:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:value: doesn't have default vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:value
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::gather_conflicts:iter: id=value
[            clap::build::app]  App::groups_for_arg: id=value
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: value, children: [] }])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:value
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:value: vals=[
    "1",
]
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="value"
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_requires:"value"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "value"=1
[         clap::parse::parser]  Parser::remove_overrides
[         clap::parse::parser]  Parser::remove_overrides:iter:file
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[         clap::parse::parser]  Parser::add_defaults:iter:file:
[         clap::parse::parser]  Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser]  Parser::add_value:iter:file: doesn't have default vals
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::validate_exclusive:iter:file
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::gather_conflicts:iter: id=file
[            clap::build::app]  App::groups_for_arg: id=file
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([Child { id: file, children: [] }])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::gather_requirements:iter:file
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[      clap::parse::validator]  Validator::validate_matched_args:iter:file: vals=[
    "file",
]
[      clap::parse::validator]  Validator::validate_arg_num_vals
[      clap::parse::validator]  Validator::validate_arg_values: arg="file"
[      clap::parse::validator]  Validator::validate_arg_values: checking validator...
[      clap::parse::validator]  good
[      clap::parse::validator]  Validator::validate_arg_requires:"file"
[      clap::parse::validator]  Validator::validate_arg_num_occurs: "file"=1
[         clap::parse::parser]  Parser::remove_overrides
[      clap::parse::validator]  Validator::validate
[         clap::parse::parser]  Parser::add_defaults
[      clap::parse::validator]  Validator::validate_conflicts
[      clap::parse::validator]  Validator::validate_exclusive
[      clap::parse::validator]  Validator::gather_conflicts
[      clap::parse::validator]  Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator]  Validator::gather_requirements
[      clap::parse::validator]  Validator::validate_required_unless
[      clap::parse::validator]  Validator::validate_matched_args
[    clap::parse::arg_matcher]  ArgMatcher::get_global_values: global_arg_vec=[]
```



---

_Label `T: bug` added by @x3rAx on 2021-03-02 11:04_

---

_Comment by @pksunkara on 2021-03-02 11:06_

Can you please run this with the master branch?

---

_Comment by @x3rAx on 2021-03-02 11:39_

I'm sorry, but I am not sure how to do this. I added
```txt
clap = { git = "https://github.com/clap-rs/clap.git", branch = "master" }
```
to `cargo.toml` and now I get the following errors and I don't understand how I can fix them (I'm new to Rust so please forgive me if this is actually pretty obvious):

```txt
error[E0308]: mismatched types
  --> src/main.rs:27:13
   |
27 |     subcmd: SubCommand,
   |             ^^^^^^^^^^ expected enum `Option`, found `&mut SubCommand`
   |
   = note:           expected enum `Option<(&str, &ArgMatches)>`
           found mutable reference `&mut SubCommand`

error[E0308]: mismatched types
  --> src/main.rs:42:13
   |
42 |     subcmd: TestSubCommand,
   |             ^^^^^^^^^^^^^^ expected enum `Option`, found `&mut TestSubCommand`
   |
   = note:           expected enum `Option<(&str, &ArgMatches)>`
           found mutable reference `&mut TestSubCommand`
```

What I tried was changing `subcmd` of the `Opts` struct to `subcmd: Option<SubCommand>,` and `subcmd: Option<(&str, &SubCommand)>,` but with no success...

---

_Comment by @pksunkara on 2021-03-02 12:30_

You would need to clone the repo locally and point to it using `path = "../clap"` instead of `git`/`branch`/`version`.

---

_Comment by @x3rAx on 2021-03-02 12:50_

From the project root, I ran

```txt
git clone https://github.com/clap-rs/clap.git
```

then changed the dependency in `cargo.toml` to

```txt
clap = { path = "./clap" }
```

but I still get the same compile error as above.

---

_Added to milestone `3.0` by @pksunkara on 2021-03-02 17:56_

---

_Label `C: derive macros` added by @pksunkara on 2021-03-09 16:57_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-03-09 16:57_

---

_Comment by @omar25h on 2021-03-10 11:12_

> I'm sorry, but I am not sure how to do this. I added
> 
> ```
> clap = { git = "https://github.com/clap-rs/clap.git", branch = "master" }
> ```
> 
> to `cargo.toml` and now I get the following errors and I don't understand how I can fix them (I'm new to Rust so please forgive me if this is actually pretty obvious):
> 
> ```
> error[E0308]: mismatched types
>   --> src/main.rs:27:13
>    |
> 27 |     subcmd: SubCommand,
>    |             ^^^^^^^^^^ expected enum `Option`, found `&mut SubCommand`
>    |
>    = note:           expected enum `Option<(&str, &ArgMatches)>`
>            found mutable reference `&mut SubCommand`
> 
> error[E0308]: mismatched types
>   --> src/main.rs:42:13
>    |
> 42 |     subcmd: TestSubCommand,
>    |             ^^^^^^^^^^^^^^ expected enum `Option`, found `&mut TestSubCommand`
>    |
>    = note:           expected enum `Option<(&str, &ArgMatches)>`
>            found mutable reference `&mut TestSubCommand`
> ```
> 
> What I tried was changing `subcmd` of the `Opts` struct to `subcmd: Option<SubCommand>,` and `subcmd: Option<(&str, &SubCommand)>,` but with no success...

I noticed that this only happens when the field is named `subcmd`. Renaming this field to anything else like `sub` would fix the compilation issue. Not sure about the reason though

---

_Comment by @pksunkara on 2021-03-10 11:45_

@omar25h Does his original code compile and it is giving the expected behaviour after that change?

If yes, in that case, we need to turn this issue into solving the `subcmd` field name problem.

---

_Comment by @omar25h on 2021-03-10 12:16_

@pksunkara The original code compiles, but the behavior is still the same as `3.0.0-beta.2`, so the issue can be reproduced.

However, the `subcmd` field name looks like a different problem. The code compiles on `3.0.0-beta.2`, but not on `master`

---

_Referenced in [clap-rs/clap#2405](../../clap-rs/clap/issues/2405.md) on 2021-03-10 18:06_

---

_Comment by @pksunkara on 2021-03-10 20:17_

Now that the error has been fixed, can someone paste what the actual behaviour is on master?

---

_Label `C: derive macros` removed by @pksunkara on 2021-03-10 20:17_

---

_Comment by @logansquirel on 2021-03-11 09:15_

> Now that the error has been fixed, can someone paste what the actual behaviour is on master?

# Actual behavior

```console
❯ tree .
.
├── Cargo.lock
├── Cargo.toml
├── src
│   └── main.rs
├── test
└── xtest

1 directory, 5 files

❯ cat src/main.rs
use std::path::PathBuf;
use clap::Clap;

fn main() {
    let _opts = Opts::parse();
}

#[derive(Clap)]
struct Opts {
    #[clap(subcommand)]
    subcmd: SubCommand,
}

#[derive(Clap)]
enum SubCommand {
    File(File),
}

#[derive(Clap)]
struct File {
    file: PathBuf,
    #[clap(subcommand)]
    subcmd: TestSubCommand,
}

#[derive(Clap)]
enum TestSubCommand {
    Test(Test),
}

#[derive(Clap)]
struct Test {
    value: i32
}

❯ cat test xtest
test file
xtest file

❯ cargo run --quiet -- file test test 1; echo $status
error: Found argument '1' which wasn't expected, or isn't valid in this context

USAGE:
    clap_issue_2375 file <file> test <value>

For more information try --help
2

❯ cargo run --quiet -- file xtest test 1; echo $status
0

❯ cargo tree | rg "clap v" | head -1
├── clap v3.0.0-beta.2 (https://github.com/clap-rs/clap#2952fd6d)
```

---

_Comment by @pksunkara on 2021-03-11 09:24_

So, the second case has been fixed (probably by @ldm0), but not the first case.

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 12:05_

---

_Label `C: parsing` added by @pksunkara on 2021-05-26 12:05_

---

_Label `:money_with_wings: $5` removed by @pksunkara on 2021-05-26 12:05_

---

_Label `:money_with_wings: $10` added by @pksunkara on 2021-05-26 12:05_

---

_Referenced in [epage/clapng#180](../../epage/clapng/issues/180.md) on 2021-12-06 21:12_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 16:31_

---

_Referenced in [jj-vcs/jj#101](../../jj-vcs/jj/issues/101.md) on 2022-03-02 18:49_

---

_Label `:money_with_wings: $10` removed by @pksunkara on 2024-06-07 06:53_

---
