---
number: 1924
title: "Missing generated help for --help and --version with `clap::App::write_help`"
type: issue
state: closed
author: SlooowAndFurious
labels:
  - C-bug
assignees: []
created_at: 2020-05-13T10:37:38Z
updated_at: 2020-05-13T11:55:32Z
url: https://github.com/clap-rs/clap/issues/1924
synced_at: 2026-01-07T13:12:19-06:00
---

# Missing generated help for --help and --version with `clap::App::write_help`

---

_Issue opened by @SlooowAndFurious on 2020-05-13 10:37_

### Code
This code shows the differences between calling this program with `--help` and displaying the help with `clap::App::write_help`.

```rust
use std::io;

extern crate clap;
use clap::{App, Arg};

fn main() {
    let app = App::new("hello")
        .version("0.1")
        .arg(
            Arg::with_name("foo")
            .help("Help about foo")
        )
        .arg(
            Arg::with_name("bar")
            .help("Help about bar")
        );

    println!("App::write_help");
    app.write_help(&mut io::stdout());

    println!("\n\n");
    println!("App::get_matches");
    app.get_matches();
}
```

### Steps to reproduce the issue

Just run `cargo run -- --help`

### Version

* **Rust**: rustc 1.43.1 (8d69840ab 2020-05-04)
* **Clap**: 2.33.1

### Actual Behavior Summary

`clap::App::write_help` writes the following to the given argument (`std::io::stdout()`)

```
hello 0.1

USAGE:
    hello [ARGS]

ARGS:
    <foo>    Help about foo
    <bar>    Help about bar
```

### Expected Behavior Summary

I think the help generated for `--help` and `--version` should be written too *or* the doc should *explicitly* warn these sections won't be written (and suggest an alternative method to do this).

The code above should show this
```
hello 0.1

USAGE:
    clap-bug [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <foo>    Help about foo
    <bar>    Help about bar

```

for both `clap::App::write_help` and `clap::App::get_matches`.

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>

App::write_help
DEBUG:clap:Help::write_app_help;
DEBUG:clap:Help::write_parser_help;
DEBUG:clap:Help::write_parser_help;
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=false
DEBUG:clap:Help::new;
DEBUG:clap:Help::write_help;
DEBUG:clap:Help::write_default_help;
DEBUG:clap:Help::write_bin_name;
hello DEBUG:clap:Help::write_version;
0.1

USAGE:DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::get_args_tag;
DEBUG:clap:usage::get_args_tag:iter:foo:
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::get_args_tag:iter: 1 Args not required or hidden
DEBUG:clap:usage::get_args_tag:iter:bar:
DEBUG:clap:Parser::groups_for_arg: name=bar
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::get_args_tag:iter: 2 Args not required or hidden
DEBUG:clap:usage::get_args_tag:iter: More than one, returning [ARGS]
DEBUG:clap:usage::create_help_usage: usage=hello [ARGS]

    hello [ARGS]

DEBUG:clap:Help::write_all_args;
ARGS:
DEBUG:clap:Help::write_args_unsorted;
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
    DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=<foo>
<foo>DEBUG:clap:Help::spec_vals: a=<foo>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
    DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
Help about foo
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
    DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=<bar>
<bar>DEBUG:clap:Help::spec_vals: a=<bar>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
    DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
Help about bar


App::get_matches
DEBUG:clap:Parser::propagate_settings: self=hello, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--help"' ([45, 45, 104, 101, 108, 112])
DEBUG:clap:Parser::is_new_arg:"--help":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--help"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--help'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --help is help or version...Help
DEBUG:clap:Parser::_help: use_long=true
DEBUG:clap:Help::write_parser_help;
DEBUG:clap:Help::write_parser_help;
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=false
DEBUG:clap:Help::new;
DEBUG:clap:Help::write_help;
DEBUG:clap:Help::write_default_help;
DEBUG:clap:Help::write_bin_name;
DEBUG:clap:Help::write_version;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::get_args_tag;
DEBUG:clap:usage::get_args_tag:iter:foo:
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::get_args_tag:iter: 1 Args not required or hidden
DEBUG:clap:usage::get_args_tag:iter:bar:
DEBUG:clap:Parser::groups_for_arg: name=bar
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::get_args_tag:iter: 2 Args not required or hidden
DEBUG:clap:usage::get_args_tag:iter: More than one, returning [ARGS]
DEBUG:clap:usage::create_help_usage: usage=clap-bug [ARGS]
DEBUG:clap:Help::write_all_args;
DEBUG:clap:Help::write_args;
DEBUG:clap:Help::write_args: Current Longest...2
DEBUG:clap:Help::write_args: New Longest...6
DEBUG:clap:Help::write_args: Current Longest...6
DEBUG:clap:Help::write_args: New Longest...9
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=--help
DEBUG:clap:Help::spec_vals: a=--help
DEBUG:clap:Help::val: Has switch...Yes
DEBUG:clap:Help::val: force_next_line...false
DEBUG:clap:Help::val: nlh...false
DEBUG:clap:Help::val: taken...21
DEBUG:clap:Help::val: help_width > (width - taken)...23 > (120 - 21)
DEBUG:clap:Help::val: longest...9
DEBUG:clap:Help::val: next_line...No
DEBUG:clap:write_spaces!: num=7
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=--version
DEBUG:clap:Help::spec_vals: a=--version
DEBUG:clap:Help::val: Has switch...Yes
DEBUG:clap:Help::val: force_next_line...false
DEBUG:clap:Help::val: nlh...false
DEBUG:clap:Help::val: taken...21
DEBUG:clap:Help::val: help_width > (width - taken)...26 > (120 - 21)
DEBUG:clap:Help::val: longest...9
DEBUG:clap:Help::val: next_line...No
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
DEBUG:clap:Help::write_args_unsorted;
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=<foo>
DEBUG:clap:Help::spec_vals: a=<foo>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=<bar>
DEBUG:clap:Help::spec_vals: a=<bar>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
hello 0.1

USAGE:
    clap-bug [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <foo>    Help about foo
    <bar>    Help about bar



</code>
</pre>
</details>


---

_Label `T: bug` added by @SlooowAndFurious on 2020-05-13 10:37_

---

_Added to milestone `3.0` by @pksunkara on 2020-05-13 10:46_

---

_Comment by @SlooowAndFurious on 2020-05-13 11:55_

Oops, not noticed issue #808 (which is closed), and actually, the doc mentions it. Sorry, I close this

---

_Closed by @SlooowAndFurious on 2020-05-13 11:55_

---
