---
number: 1915
title: v3 App Options no longer work on subcommand arguments
type: issue
state: closed
author: publicarray
labels:
  - C-bug
assignees: []
created_at: 2020-05-08T02:41:05Z
updated_at: 2020-05-08T10:35:25Z
url: https://github.com/clap-rs/clap/issues/1915
synced_at: 2026-01-10T01:27:09Z
---

# v3 App Options no longer work on subcommand arguments

---

_Issue opened by @publicarray on 2020-05-08 02:41_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```rust
use clap::{App, Arg, AppSettings};

fn main() {
    let cli = App::new("foo")
        .setting(AppSettings::SubcommandRequired)
        .arg(Arg::with_name("v")
                .short('v')
                .multiple(true)
                .about("Sets the level of verbosity"))
        .subcommand(App::new("bar")
                .arg(Arg::with_name("baz")
                        .long("baz")
                    )).get_matches();

    let log_level = cli.occurrences_of("v");
    println!("{}", log_level);
    if let Some(bar) = cli.subcommand_matches("bar") {
        if bar.is_present("baz") {
            println!("Hello World");
        }
    }
}

```

### Steps to reproduce the issue

1. Run `cargo run -- -v bar --baz` error: Found argument '--baz' which wasn't expected, or isn't valid in this context

### Version

* **Rust**: rustc 1.44.0-nightly
* **Clap**: master

### Actual Behavior Summary

1. Run `cargo run -- bar` = 0 (expected)
2. Run `cargo run -- bar --baz` = 0 Hello World (expected)
3. Run `cargo run -- -v bar` = 1  (expected)
4. Run `cargo run -- -v bar --baz` error: Found argument '--baz' which wasn't expected, or isn't valid in this context

I'm using clap-rs in https://github.com/publicarray/BunnyCLI

### Expected Behavior Summary

From run number 4 the output should be:

```
1
Hello World
```

### Additional context


### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

     Running `target/debug/my-test -v bar --baz`
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:foo
[            clap::build::app] 	App::_derive_display_order:bar
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_create_help_and_version: Building help
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:v
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[         clap::parse::parser] 	Parser::_verify_positionals
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"-v"' ([45, 118])
[         clap::parse::parser] 	Parser::is_new_arg: "-v":NotFound
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: - found
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser] 	Parser::possible_subcommand: arg="-v"
[         clap::parse::parser] 	Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser] 	Parser::parse_short_arg: full_arg="-v"
[         clap::parse::parser] 	Parser::parse_short_arg:iter:v
[         clap::parse::parser] 	Parser::parse_short_arg:iter:v: Found valid opt or flag
[          clap::util::argstr] 	ArgSplit::next: self=ArgSplit { sep: [118, 0, 0, 0], sep_len: 1, val: [118], pos: 0 }
[         clap::parse::parser] 	Parser::parse_short_arg:iter:v: i=1, arg_os="v"
[         clap::parse::parser] 	Parser::parse_opt; opt=v, val=None
[         clap::parse::parser] 	Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE_OCC | TAKES_VAL | DELIM_NOT_SET | MULTIPLE_VALS)
[         clap::parse::parser] 	Parser::parse_opt; Checking for val...
[         clap::parse::parser] 	None
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of: arg=v
[    clap::parse::arg_matcher] 	ArgMatcher::inc_occurrence_of: first instance
[         clap::parse::parser] 	groups_for_arg: name=v
[         clap::parse::parser] 	Parser::parse_opt: More arg vals required...
[         clap::parse::parser] 	Parser::get_matches_with: After parse_short_arg Opt(v)
[         clap::parse::parser] 	Parser::maybe_inc_pos_counter: arg = v
[         clap::parse::parser] 	Parser::maybe_inc_pos_counter: is it positional?
[         clap::parse::parser] 	No
[         clap::parse::parser] 	groups_for_arg: name=v
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"bar"' ([98, 97, 114])
[         clap::parse::parser] 	Parser::is_new_arg: "bar":Opt(v)
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: probably value
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=false
[         clap::parse::parser] 	Parser::add_val_to_arg; arg=v, val="bar"
[         clap::parse::parser] 	Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."bar"
[         clap::parse::parser] 	groups_for_arg: name=v
[    clap::parse::arg_matcher] 	ArgMatcher::needs_more_vals: o=v
[         clap::parse::parser] 	Parser::get_matches_with: Begin parsing '"--baz"' ([45, 45, 98, 97, 122])
[         clap::parse::parser] 	Parser::is_new_arg: "--baz":Opt(v)
[         clap::parse::parser] 	Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser] 	Parser::is_new_arg: -- found
[         clap::parse::parser] 	Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser] 	Parser::parse_long_arg
[         clap::parse::parser] 	Parser::parse_long_arg: Does it contain '='...
[         clap::parse::parser] 	No
[         clap::parse::parser] 	Parser::parse_long_arg: Didn't match anything
[         clap::parse::parser] 	Parser::did_you_mean_error: arg=baz
[         clap::parse::parser] 	Parser::did_you_mean_error: longs=["help", "version"]
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_derive_display_order:bar
[            clap::build::app] 	App::_create_help_and_version
[            clap::build::app] 	App::_create_help_and_version: Building --help
[            clap::build::app] 	App::_create_help_and_version: Building --version
[            clap::build::app] 	App::_debug_asserts
[            clap::build::arg] 	Arg::_debug_asserts:baz
[            clap::build::arg] 	Arg::_debug_asserts:help
[            clap::build::arg] 	Arg::_debug_asserts:version
[         clap::output::usage] 	Usage::create_usage_with_title
[         clap::output::usage] 	Usage::create_usage_no_title
[         clap::output::usage] 	Usage::create_smart_usage
[         clap::output::usage] 	Usage::get_required_usage_from: incls=[v], matcher=false, incl_last=true
[         clap::output::usage] 	Usage::get_required_usage_from:iter:v
[         clap::output::usage] 	Usage::get_required_usage_from: ret_val=["-v <v>..."]
[            clap::build::app] 	App::color: Color setting...
[            clap::build::app] 	Auto
[           clap::output::fmt] 	is_a_tty: stderr=true
error: Found argument '--baz' which wasn't expected, or isn't valid in this context

	Did you mean to put '--baz' after the subcommand 'bar'?

If you tried to supply `--baz` as a PATTERN use `-- --baz`

USAGE:
    my-test -v <v>...

For more information try --help


</code>
</pre>
</details>


---

_Label `T: bug` added by @publicarray on 2020-05-08 02:41_

---

_Comment by @CreepySkeleton on 2020-05-08 03:08_

This is because `bar` is considered as a value for `-v` instead of subcommand. See https://github.com/clap-rs/clap/issues/1721, https://github.com/clap-rs/clap/issues/1031, and https://github.com/clap-rs/clap/pull/1834. 

TL;DR: use `SubcommandPrecedenceOverArg` settings.

Personally, I would recommend you to redesign the CLI. This kind of "values vs subcommand" may cause more harm than goon in long term.

---

_Closed by @CreepySkeleton on 2020-05-08 03:09_

---

_Comment by @publicarray on 2020-05-08 03:36_

Thanks very @CreepySkeleton, that works nicely & Thanks for the advice. Any ideas on how to design it better ?

> goon

I'm going to have some wine now 🤣

---

_Comment by @CreepySkeleton on 2020-05-08 04:57_

Well, if you truly intend `-v` to take value, I recommend to go `cargo --features "f1 f2"` way. Can be achieved via [`value_delimiter(" ")`](https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.value_delimiter) and [`require_delimiter(true)`](https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.require_delimiter). 

But I bet your intention was to allow multiple number of occurrences, not values. `multiple(true)` means _both_ multiple values (requiring at least one value) and multiple occurrences while you only need the latter. Use `multiple_occurrences(true)` and you'll live happily ever after.

> goon

I must have been a comedian in a previous life, don't you think? Even when my mind isn't up to it, my muscle memory knows better and bony fingers hit the right button.

Unfortunately, I can't share the wine with you. Skeletons don't have much of taste, after all.

---

_Comment by @publicarray on 2020-05-08 09:54_

Thank you! I'm going to pick live happily ever after `multiple_occurrences(true)`. Your comment does have lots of useful information and I did not find this in the manual. Maybe this can be included or maybe I need to get my eye sight checked.

Haha, well played..

True, You could give it go but it might stain your ribs: https://www.youtube.com/watch?v=1fzXmJyolfY&t=33

---

_Comment by @pksunkara on 2020-05-08 10:35_

We are still working on changelog and documentation. You are not supposed to use v3 yet.

---
