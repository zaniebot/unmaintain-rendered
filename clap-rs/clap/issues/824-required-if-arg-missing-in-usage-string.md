```yaml
number: 824
title: required_if arg missing in usage string
type: issue
state: closed
author: F0x06
labels:
  - C-bug
assignees: []
created_at: 2017-01-27T18:47:47Z
updated_at: 2018-08-02T03:30:00Z
url: https://github.com/clap-rs/clap/issues/824
synced_at: 2026-01-12T16:14:09Z
```

# required_if arg missing in usage string

---

_@F0x06_

Hi, first thank you for this awesome lib !

### Rust Version

* rustc 1.14.0 (e8a012324 2016-12-16)

### Affected Version of clap

* 2.20.0

### Expected Behavior Summary

```
$./clap_test --input somepath --target file

error: The following required arguments were not provided:
   --output <output>

USAGE:
    clap_test [OPTIONS] --target <target> --input <input> --output <output>

For more information try --help
```


### Actual Behavior Summary

```
$./clap_test --input somepath --target file

error: The following required arguments were not provided:

USAGE:
    clap_test [OPTIONS] --target <target> --input <input>

For more information try --help
```

### Steps to Reproduce the issue

* Use the code bellow

### Sample Code or Link to Sample Code

```rust
extern crate clap;

use clap::{Arg, App};

fn main() {
    App::new("Test app")
        .version("1.0")
        .author("F0x06")
        .about("Arg test")
        .arg(Arg::with_name("target")
            .takes_value(true)
            .required(true)
            .possible_values(&["file", "stdout"])
            .long("target"))
        .arg(Arg::with_name("input")
            .takes_value(true)
            .required(true)
            .long("input"))
        .arg(Arg::with_name("output")
            .takes_value(true)
            .required_if("target", "file")
            .long("output"))
        .get_matches();
}
```

### Debug output

```
$./clap_test --input somepath --target file

DEBUG:clap:Parser::propogate_settings;
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--input"' ([45, 45, 105, 110, 112, 117, 116])
DEBUG:clap:Parser::is_new_arg: arg="--input", Needs Val of=None
DEBUG:clap:Parser::is_new_arg: Does arg allow leading hyphen...false
DEBUG:clap:Parser::is_new_arg: Starts new arg...--
DEBUG:clap:Parser::is_new_arg: Starts new arg...true
DEBUG:clap:Parser::possible_subcommand: arg="--input"
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--input <input>'
DEBUG:clap:Parser::parse_opt;
DEBUG:clap:validate_multiples!;
DEBUG:clap:Parser::parse_optChecking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=input
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=input
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Is '--input <input>' in overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--input <input>' have overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--input <input>' have conflicts...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--input <input>' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"somepath"' ([115, 111, 109, 101, 112, 97, 116, 104])
DEBUG:clap:Parser::is_new_arg: arg="somepath", Needs Val of=Some("input")
DEBUG:clap:Parser::is_new_arg: Does arg allow leading hyphen...false
DEBUG:clap:Parser::is_new_arg: Starts new arg...Probable value
DEBUG:clap:Parser::is_new_arg: Starts new arg...false
DEBUG:clap:Parser::possible_subcommand: arg="somepath"
DEBUG:clap:Parser::add_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."somepath"
DEBUG:clap:Parser::groups_for_arg: name=input
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::validate_value: val="somepath"
DEBUG:clap:ArgMatcher::needs_more_vals: o=input
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--target"' ([45, 45, 116, 97, 114, 103, 101, 116])
DEBUG:clap:Parser::is_new_arg: arg="--target", Needs Val of=None
DEBUG:clap:Parser::is_new_arg: Does arg allow leading hyphen...false
DEBUG:clap:Parser::is_new_arg: Starts new arg...--
DEBUG:clap:Parser::is_new_arg: Starts new arg...true
DEBUG:clap:Parser::possible_subcommand: arg="--target"
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--target <target>'
DEBUG:clap:Parser::parse_opt;
DEBUG:clap:validate_multiples!;
DEBUG:clap:Parser::parse_optChecking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=target
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=target
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Is '--target <target>' in overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--target <target>' have overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--target <target>' have conflicts...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '--target <target>' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"file"' ([102, 105, 108, 101])
DEBUG:clap:Parser::is_new_arg: arg="file", Needs Val of=Some("target")
DEBUG:clap:Parser::is_new_arg: Does arg allow leading hyphen...false
DEBUG:clap:Parser::is_new_arg: Starts new arg...Probable value
DEBUG:clap:Parser::is_new_arg: Starts new arg...false
DEBUG:clap:Parser::possible_subcommand: arg="file"
DEBUG:clap:Parser::add_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."file"
DEBUG:clap:Parser::groups_for_arg: name=target
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::validate_value: val="file"
DEBUG:clap:ArgMatcher::needs_more_vals: o=target
DEBUG:clap:Parser::validate;
DEBUG:clap:Parser::validate_blacklist: blacklist=[]
DEBUG:clap:Parser::validate_required: required=["target", "input"];
DEBUG:clap:Parser::validate_required:iter: name=target
DEBUG:clap:Parser::validate_required:iter: name=input
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:Parser::get_required_from; reqs=["target", "input"]
DEBUG:clap:Parser::get_required_from: args_in_groups=[]
DEBUG:clap:Parser::create_usage;
DEBUG:clap:Parser::create_usage_no_title;
DEBUG:clap:Parser::get_required_from; reqs=["target", "input"]
DEBUG:clap:Parser::get_required_from: args_in_groups=[]
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:Parser::needs_flags_tag;
DEBUG:clap:Parser::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:Parser::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:Parser::needs_flags_tag: [FLAGS] not required
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:Colorizer::error;
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::good;
DEBUG:clap:is_a_tty: stderr=true
error: The following required arguments were not provided:

USAGE:
    clap_test [OPTIONS] --target <target> --input <input>

For more information try --help
```

### Additional questions

* Can i use clap required_if with a default value argument (unspecified argument) ? for example:

```rust
fn main() {
    App::new("Test app")
        .version("1.0")
        .author("F0x06")
        .about("Arg test")
        .arg(Arg::with_name("target")
            .takes_value(true)
            .possible_values(&["file", "stdout"])
            .default_value("file")
            .long("target"))
        .arg(Arg::with_name("input")
            .takes_value(true)
            .required(true)
            .long("input"))
        .arg(Arg::with_name("output")
            .takes_value(true)
            .required_if("target", "file")
            .long("output"))
        .get_matches();
}
```


---

_Comment by @kbknapp on 2017-01-30 02:06_

Thanks for filing this! Sorry I've been traveling the past few days. This should be a simple fix, I've got a few other issues to knock out first, but I should be able to get to this one this week :wink:

> Hi, first thank you for this awesome lib !

Thanks 😄 

> Can i use clap required_if with a default value argument (unspecified argument) ?

Currently no. I'll file a separate issue to track this bug :wink:


---

_Label `C: errors` added by @kbknapp on 2017-01-30 02:07_

---

_Label `D: easy` added by @kbknapp on 2017-01-30 02:07_

---

_Label `P3: want to have` added by @kbknapp on 2017-01-30 02:07_

---

_Label `T: bug` added by @kbknapp on 2017-01-30 02:07_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-30 02:07_

---

_Referenced in [clap-rs/clap#831](../../clap-rs/clap/issues/831.md) on 2017-01-30 02:10_

---

_Added to milestone `2.20.2` by @kbknapp on 2017-01-30 04:36_

---

_Renamed from "Wrong error output with required_if" to "required_if arg missing in usage string" by @kbknapp on 2017-01-30 04:40_

---

_Comment by @F0x06 on 2017-01-30 06:55_

> Thanks for filing this! Sorry I've been traveling the past few days. This should be a simple fix, I've got a few other issues to knock out first, but I should be able to get to this one this week

No problem. awesome, thank you ! :)
> Currently no. I'll file a separate issue to track this bug

Awesome and thank you again ! :)

---

_Added to milestone `2.20.3` by @kbknapp on 2017-02-03 16:13_

---

_Removed from milestone `2.20.2` by @kbknapp on 2017-02-03 16:13_

---

_Comment by @kbknapp on 2017-02-03 22:45_

v2.20.3 was just uploaded to crates.io and contains this fix.

---

_Closed by @kbknapp on 2017-02-03 22:45_

---
