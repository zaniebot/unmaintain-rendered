---
number: 1046
title: Subcommands help header displayed with no visible subcommands
type: issue
state: closed
author: jonhoo
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2017-09-14T14:52:33Z
updated_at: 2018-08-02T03:30:11Z
url: https://github.com/clap-rs/clap/issues/1046
synced_at: 2026-01-07T13:12:19-06:00
---

# Subcommands help header displayed with no visible subcommands

---

_Issue opened by @jonhoo on 2017-09-14 14:52_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.22.0-nightly (ddd123ed9 2017-09-09)

### Affected Version of clap

2.26.1

### Expected Behavior Summary

If there are no visible subcommands, the `SUBCOMMANDS` section in help output should not be displayed. Similarly, the `help` subcommand should not need to be hidden if there are no visible subcommands.

### Actual Behavior Summary

The `SUBCOMMANDS` section is always displayed when any subcommands are defined (even if they have `AppSettings::Hidden`). The `help` subcommand is not automatically hidden if there are no other visible subcommands.

### Steps to Reproduce the issue

Create an `App` with a subcommand that has been hidden using `AppSettings::Hidden`. Hide the `help` subcommand with `AppSettings::DisableHelpSubcommand`.

### Sample Code or Link to Sample Code

```rust
App::new("time")
    .setting(AppSettings::DisableHelpSubcommand)
    .subcommand(SubCommand::with_name("cmd")
        .setting(AppSettings::Hidden))
```

### Debug output

Compile clap with cargo features `"debug"` such as:

```console
DEBUG:clap:Parser::add_subcommand: term_w=None, name=cmd
DEBUG:clap:Parser::propogate_settings: self=time, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::propogate_settings: sc=cmd, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | HIDDEN | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::propogate_settings: self=cmd, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--help"' ([45, 45, 104, 101, 108, 112])
DEBUG:clap:Parser::is_new_arg: arg="--help", Needs Val of=NotFound
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--help"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--help'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --help is help or version...Help
DEBUG:clap:Parser::_help: use_long=true
DEBUG:clap:Parser::use_long_help: ret=false
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
DEBUG:clap:usage::create_help_usage: usage=x
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
DEBUG:clap:Help::val: help_width > (width - taken)...23 > (149 - 21)
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
DEBUG:clap:Help::val: help_width > (width - taken)...26 > (149 - 21)
DEBUG:clap:Help::val: longest...9
DEBUG:clap:Help::val: next_line...No
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
DEBUG:clap:Help::write_subcommands;
time

USAGE:
    x

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:

```

---

_Label `C: help message` added by @kbknapp on 2017-09-14 14:53_

---

_Label `C: subcommands` added by @kbknapp on 2017-09-14 14:53_

---

_Label `D: easy` added by @kbknapp on 2017-09-14 14:53_

---

_Label `P3: want to have` added by @kbknapp on 2017-09-14 14:53_

---

_Label `T: enhancement` added by @kbknapp on 2017-09-14 14:53_

---

_Label `W: 2.x` added by @kbknapp on 2017-09-14 14:53_

---

_Closed by @kbknapp on 2017-09-15 18:09_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-10-04 21:14_

---
