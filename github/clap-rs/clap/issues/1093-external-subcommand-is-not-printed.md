---
number: 1093
title: External subcommand is not printed 
type: issue
state: closed
author: marmistrz
labels:
  - C-bug
  - A-help
  - E-medium
  - E-easy
assignees: []
created_at: 2017-11-05T16:34:01Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1093
synced_at: 2026-01-07T13:12:19-06:00
---

# External subcommand is not printed 

---

_Issue opened by @marmistrz on 2017-11-05 16:34_

### Rust Version

* `1.21`

### Affected Version of clap

`2.27.1`

### Actual Behavior Summary
Take the following code. Launching `./executable --help` displays the syntax to be
```
    ./executable [OPTIONS]
```

### Expected Behavior Summary
Something like:
```
    ./executable [OPTIONS] [COMMAND]
```
How can the user know that there's a subcommand to be accepted?

### Sample Code or Link to Sample Code


### Debug output
```
DEBUG:clap:Parser::propagate_settings: self=app, g_settings=AppFlags(
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
DEBUG:clap:usage::create_help_usage: usage=clap-test [OPTIONS]
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
DEBUG:clap:Help::write_args;
DEBUG:clap:Help::write_args: Current Longest...2
DEBUG:clap:OptBuilder::fmt:option
DEBUG:clap:Help::write_args: New Longest...14
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=DEBUG:clap:OptBuilder::fmt:option
-o <option>...
DEBUG:clap:Help::spec_vals: a=DEBUG:clap:OptBuilder::fmt:option
-o <option>...
DEBUG:clap:Help::val: Has switch...Yes
DEBUG:clap:Help::val: force_next_line...false
DEBUG:clap:Help::val: nlh...false
DEBUG:clap:Help::val: taken...26
DEBUG:clap:Help::val: help_width > (width - taken)...0 > (120 - 26)
DEBUG:clap:Help::val: longest...14
DEBUG:clap:Help::val: next_line...No
DEBUG:clap:OptBuilder::fmt:option
DEBUG:clap:write_spaces!: num=8
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
```

---

_Referenced in [clap-rs/clap#1012](../../clap-rs/clap/pulls/1012.md) on 2017-11-05 16:37_

---

_Comment by @kbknapp on 2017-11-06 03:37_

Good catch, thanks for filing this bug! Should be a super easy fix if anyone wants a good first issue.

Relevant lines are in [src/app/parser.rs#L475-L477](https://github.com/kbknapp/clap-rs/blob/284d01bded1970416e299ccc2e9e8309c40e278a/src/app/parser.rs#L475-L477).

One should really only need to add `|| self.is_set(AS::AllowExternalSubcommands)` to that return.

---

_Label `C: help message` added by @kbknapp on 2017-11-06 03:38_

---

_Label `C: subcommands` added by @kbknapp on 2017-11-06 03:38_

---

_Label `D: easy` added by @kbknapp on 2017-11-06 03:38_

---

_Label `good first issue` added by @kbknapp on 2017-11-06 03:38_

---

_Label `M: mentored` added by @kbknapp on 2017-11-06 03:38_

---

_Label `P3: want to have` added by @kbknapp on 2017-11-06 03:38_

---

_Label `T: bug` added by @kbknapp on 2017-11-06 03:38_

---

_Label `W: 2.x` added by @kbknapp on 2017-11-06 03:38_

---

_Added to milestone `2.27.2` by @kbknapp on 2017-11-06 03:39_

---

_Closed by @kbknapp on 2017-11-07 02:48_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-13 01:54_

---
