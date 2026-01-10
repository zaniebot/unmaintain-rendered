---
number: 1487
title: Required group can duplicate items in help text
type: issue
state: closed
author: TyPR124
labels: []
assignees: []
created_at: 2019-06-12T03:45:30Z
updated_at: 2019-06-26T00:11:49Z
url: https://github.com/clap-rs/clap/issues/1487
synced_at: 2026-01-10T01:26:55Z
---

# Required group can duplicate items in help text

---

_Issue opened by @TyPR124 on 2019-06-12 03:45_

### Affected Version of clap

* 2.33.0, and master branch as of time of writing (784524f)

### Bug Summary

Code below outputs the following in help
`USAGE:`
`    test <arg1|arg2|arg1|arg2>`

The code makes the undocumented mistake of including the group in both args, and also both args in the group. I feel this should be allowed, and the duplicates just removed.

### Expected Behavior Summary
`USAGE:`
`    test <arg1|arg2>`


### Sample Code or Link to Sample Code
Issue visible by running without arguments or with -h

https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=e0592c09f4648ffb8ea1a81d8ebc074d

```
use clap::{App, Arg, ArgGroup};

fn main() {
    let _ = App::new("test")
        .arg(Arg::with_name("arg1")
            .group("group1"))
        .arg(Arg::with_name("arg2")
            .group("group1"))
        .group(ArgGroup::with_name("group1")
            .args(&["arg1", "arg2"])
            .required(true))
        .get_matches();
}
```

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>

DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-h"' ([45, 104])
DEBUG:clap:Parser::is_new_arg:"-h":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-h"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-h"
DEBUG:clap:Parser::parse_short_arg:iter:h
DEBUG:clap:Parser::parse_short_arg:iter:h: Found valid flag
DEBUG:clap:Parser::check_for_help_and_version_char;
DEBUG:clap:Parser::check_for_help_and_version_char: Checking if -h is help or version...Help
DEBUG:clap:Parser::_help: use_long=false
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
DEBUG:clap:usage::get_required_usage_from: reqs=["group1"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["group1"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=["arg1", "arg2"]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::get_args_tag;
DEBUG:clap:usage::get_args_tag:iter:arg1:
DEBUG:clap:Parser::groups_for_arg: name=arg1
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
        Found 'group1'
        Found 'group1'
DEBUG:clap:usage::get_args_tag:iter:arg1:iter:group1;
DEBUG:clap:usage::get_args_tag:iter:arg2:
DEBUG:clap:Parser::groups_for_arg: name=arg2
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
        Found 'group1'
        Found 'group1'
DEBUG:clap:usage::get_args_tag:iter:arg2:iter:group1;
DEBUG:clap:usage::create_help_usage: usage=clap-testing.exe <arg1|arg2|arg1|arg2>
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
DEBUG:clap:Help::val: arg=<arg1>
DEBUG:clap:Help::spec_vals: a=<arg1>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
DEBUG:clap:Help::write_arg;
DEBUG:clap:Help::short;
DEBUG:clap:Help::long;
DEBUG:clap:Help::val: arg=<arg2>
DEBUG:clap:Help::spec_vals: a=<arg2>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No

</code>
</pre>
</details>


---

_Referenced in [clap-rs/clap#1511](../../clap-rs/clap/pulls/1511.md) on 2019-06-26 00:05_

---

_Comment by @TyPR124 on 2019-06-26 00:10_

Fixed in v3 - closing issue
Also added regression test in #1511

---

_Closed by @TyPR124 on 2019-06-26 00:10_

---

_Referenced in [doy/rbw#54](../../doy/rbw/issues/54.md) on 2021-04-17 06:39_

---
