```yaml
number: 2125
title: Excess whitespace in help when using default and possible values
type: issue
state: closed
author: ghost
labels:
  - C-bug
assignees: []
created_at: 2020-09-06T14:40:40Z
updated_at: 2020-09-08T17:54:27Z
url: https://github.com/clap-rs/clap/issues/2125
synced_at: 2026-01-12T16:14:12Z
```

# Excess whitespace in help when using default and possible values

---

_@ghost_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

```rust
use clap::Clap;

#[derive(Debug, Clap)]
struct Opts {
    /// testing help output
    #[clap(short, long, default_value = "foo", possible_values = &["foo", "bar", "baz"])]
    myarg: String,
}

fn main() {
    let _ = Opts::parse();
}
```

### Steps to reproduce the issue

1. Run `cargo build`
2. Execute compiled binary with the `-h` or `--help` flags to print the help text

### Version

* **Rust**: `rustc 1.46.0 (04488afe3 2020-08-24)`
* **Clap**: `3.0.0-beta.1`, tried `v3-dev` branch but ended up with a very large number of compiler errors unrelated to my code

### Actual Behavior Summary

When checking the help an extra space is present in the output between the text for the default and possible values, snippit below.

```
OPTIONS:
    -m, --myarg <myarg>    testing help output [default: foo]  [possible values: foo, bar, baz]
```

### Expected Behavior Summary

It is subtle but one less space is expected between the default and possible values text.

```
OPTIONS:
    -m, --myarg <myarg>    testing help output [default: foo] [possible values: foo, bar, baz]
```

### Additional context

The same issue is in the current structopt version, but as it is being merged here I figured it was better to report here.

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

[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_derive_display_order:demo-rs
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:myarg
[            clap::build::arg]  Arg::_debug_asserts:help
[            clap::build::arg]  Arg::_debug_asserts:version
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::_build
[         clap::parse::parser]  Parser::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing '"-h"' ([45, 104])
[         clap::parse::parser]  Parser::is_new_arg: "-h":NotFound
[         clap::parse::parser]  Parser::is_new_arg: arg_allows_tac=false
[         clap::parse::parser]  Parser::is_new_arg: - found
[         clap::parse::parser]  Parser::is_new_arg: starts_new_arg=true
[         clap::parse::parser]  Parser::possible_subcommand: arg="-h"
[         clap::parse::parser]  Parser::get_matches_with: possible_sc=false, sc=None
[         clap::parse::parser]  Parser::parse_short_arg: full_arg="-h"
[         clap::parse::parser]  Parser::parse_short_arg:iter:h
[         clap::parse::parser]  Parser::parse_short_arg:iter:h: Found valid opt or flag
[         clap::parse::parser]  Parser::check_for_help_and_version_char
[         clap::parse::parser]  Parser::check_for_help_and_version_char: Checking if -h is help or version...
[         clap::parse::parser]  Help
[         clap::parse::parser]  Parser::help_err: use_long=false
[         clap::parse::parser]  Parser::color_help
[           clap::output::fmt]  is_a_tty: stderr=false
[          clap::output::help]  Help::new
[          clap::output::help]  Help::write_help
[          clap::output::help]  Help::write_default_help
[          clap::output::help]  Help::write_bin_name
[          clap::output::help]  Help::write_version
[         clap::output::usage]  Usage::create_usage_no_title
[         clap::output::usage]  Usage::create_help_usage; incl_reqs=true
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[         clap::output::usage]  Usage::needs_flags_tag
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=help
[         clap::output::usage]  Usage::needs_flags_tag:iter: f=version
[         clap::output::usage]  Usage::needs_flags_tag: [FLAGS] not required
[         clap::output::usage]  Usage::create_help_usage: usage=demo-rs.exe [OPTIONS]
[          clap::output::help]  Help::write_all_args
[          clap::output::help]  should_show_arg: use_long=false, arg=myarg
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=help
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...6
[          clap::output::help]  should_show_arg: use_long=false, arg=version
[          clap::output::help]  Help::write_args: Current Longest...6
[          clap::output::help]  Help::write_args: New Longest...9
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=help
[          clap::output::help]  Help::spec_vals: a=--help
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...21
[          clap::output::help]  val: help_width > (width - taken)...23 > (120 - 21)
[          clap::output::help]  Help::val: longest...9
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=7
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=version
[          clap::output::help]  Help::spec_vals: a=--version
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...21
[          clap::output::help]  val: help_width > (width - taken)...26 > (120 - 21)
[          clap::output::help]  Help::val: longest...9
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=4
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No
[          clap::output::help]  Help::write_args
[          clap::output::help]  should_show_arg: use_long=false, arg=myarg
[          clap::output::help]  Help::write_args: Current Longest...2
[          clap::output::help]  Help::write_args: New Longest...15
[          clap::output::help]  Help::write_arg
[          clap::output::help]  Help::short
[          clap::output::help]  Help::long
[          clap::output::help]  Help::val: arg=myarg
[          clap::output::help]  Help::spec_vals: a=--myarg <myarg>
[          clap::output::help]  Help::spec_vals: Found default value...[["foo"]]
[          clap::output::help]  Help::spec_vals: Found possible vals...["foo", "bar", "baz"]
[          clap::output::help]  Help::val: Has switch...
[          clap::output::help]  Yes
[          clap::output::help]  Help::val: force_next_line...false
[          clap::output::help]  Help::val: nlh...false
[          clap::output::help]  Help::val: taken...27
[          clap::output::help]  val: help_width > (width - taken)...68 > (120 - 27)
[          clap::output::help]  Help::val: longest...15
[          clap::output::help]  Help::val: next_line...
[          clap::output::help]  No
[          clap::output::help]  Help::write_nspaces!: num=4
[          clap::output::help]  Help::help
[          clap::output::help]  Help::help: Next Line...false
[          clap::output::help]  Help::help: Too long...
[          clap::output::help]  No

</code>
</pre>
</details>


---

_Label `T: bug` added by @ghost on 2020-09-06 14:40_

---

_Comment by @pksunkara on 2020-09-06 17:12_

This has been fixed in master branch by #2109. Can you please confirm?

---

_Closed by @pksunkara on 2020-09-06 17:12_

---

_Comment by @ghost on 2020-09-08 17:54_

I didn't see this soon enough, but it does look like it is fixed on `master`. Output below using the `master` branch in my `Cargo.toml`.

```
OPTIONS:
    -m, --myarg <myarg>    testing help output [default: foo] [possible values: foo, bar, baz]
```

---
