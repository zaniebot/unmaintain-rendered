---
number: 1047
title: Incorrect default value behavior with require_equals and min_values(0)
type: issue
state: closed
author: jonhoo
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2017-09-14T15:01:24Z
updated_at: 2018-08-02T03:30:11Z
url: https://github.com/clap-rs/clap/issues/1047
synced_at: 2026-01-10T01:26:42Z
---

# Incorrect default value behavior with require_equals and min_values(0)

---

_Issue opened by @jonhoo on 2017-09-14 15:01_

### Rust Version

rustc 1.22.0-nightly (ddd123ed9 2017-09-09)

### Affected Version of clap

2.26.1

### Expected Behavior Summary

When a `require_equals` + `min_values(0)` argument is given on the command line without a value, the default value should be set as its value in matches.

### Actual Behavior Summary

If the argument is given without a value, `occurs` is 1, but `vals` is `[]`.

### Steps to Reproduce the issue

Using the code below, try the following command lines:
```console
$ foo
occurs: 0
value: Some(",")
$ foo -d=@
occurs: 1
value: Some("@")
$ foo -d
occurs: 1
value: None
```

Notice that in the last case, `value` should be `Some(",")`.

### Sample Code or Link to Sample Code

```rust
extern crate clap;

use clap::{App, Arg};
fn main() {
    let m = App::new("foo")
        .arg(
            Arg::with_name("delimiter")
                .short("d")
                .long("delimiter")
                .takes_value(true)
                .require_equals(true)
                .min_values(0)
                .default_value(","),
        )
        .get_matches();
    println!("occurs: {}", m.occurrences_of("delimiter"));
    println!("value: {:?}", m.value_of("delimiter"));
}
```

### Debug output

For the `-d` case:
```console
DEBUG:clap:Parser::propogate_settings: self=time, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-d"' ([45, 100])
DEBUG:clap:Parser::is_new_arg: arg="-d", Needs Val of=NotFound
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-d"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-d"
DEBUG:clap:Parser::parse_short_arg:iter:d
DEBUG:clap:Parser::parse_short_arg:iter:d: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:d: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=delimiter, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL | DELIM_NOT_SET | REQUIRE_EQUALS)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=delimiter
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=delimiter
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals not required...
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt:delimiter
DEBUG:clap:arg_post_processing!: Is '--delimiter=<delimiter>' in overrides...No
DEBUG:clap:OptBuilder::fmt:delimiter
DEBUG:clap:arg_post_processing!: Does '--delimiter=<delimiter>' have overrides...No
DEBUG:clap:OptBuilder::fmt:delimiter
DEBUG:clap:arg_post_processing!: Does '--delimiter=<delimiter>' have conflicts...No
DEBUG:clap:OptBuilder::fmt:delimiter
DEBUG:clap:arg_post_processing!: Does '--delimiter=<delimiter>' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Parser:get_matches_with: After parse_short_arg ValuesDone
DEBUG:clap:Validator::validate;
DEBUG:clap:Validator::validate_blacklist: blacklist=[]
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:delimiter: vals=[]
DEBUG:clap:Validator::validate_arg_num_vals;
DEBUG:clap:Validator::validate_arg_num_vals: min_vals set: 0
DEBUG:clap:Validator::validate_values: arg="delimiter"
DEBUG:clap:Validator::validate_arg_requires;
DEBUG:clap:Validator::validate_arg_num_occurs: a=delimiter;
DEBUG:clap:usage::create_usage_with_title;
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
DEBUG:clap:usage::create_help_usage: usage=x [OPTIONS]
```

---

_Label `C: options` added by @kbknapp on 2017-09-14 15:02_

---

_Label `C: parsing` added by @kbknapp on 2017-09-14 15:02_

---

_Label `D: easy` added by @kbknapp on 2017-09-14 15:02_

---

_Label `P2: need to have` added by @kbknapp on 2017-09-14 15:02_

---

_Label `T: bug` added by @kbknapp on 2017-09-14 15:02_

---

_Label `W: 2.x` added by @kbknapp on 2017-09-14 15:02_

---

_Comment by @jonhoo on 2017-09-14 15:03_

And because @kbknapp raised it during discussion: adding another dummy argument and passing it after `-d` (as in `-d -x`) does not change this outcome.

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-09-14 17:09_

---

_Closed by @kbknapp on 2017-09-15 18:09_

---

_Referenced in [clap-rs/clap#2172](../../clap-rs/clap/pulls/2172.md) on 2020-10-14 07:47_

---
