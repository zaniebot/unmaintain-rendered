```yaml
number: 1586
title: requires_all ArgGroup with a default, causes the entire ArgGroup to be required
type: issue
state: closed
author: leeola
labels:
  - C-bug
  - E-help-wanted
assignees: []
created_at: 2019-10-26T21:14:07Z
updated_at: 2021-10-27T21:31:06Z
url: https://github.com/clap-rs/clap/issues/1586
synced_at: 2026-01-12T16:14:11Z
```

# requires_all ArgGroup with a default, causes the entire ArgGroup to be required

---

_@leeola_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.40.0-nightly (10a52c25c 2019-10-24)

### Affected Version of clap

clap 2.33.0

### Expected Behavior Summary

1. Using `ArgGroup` to make 3 flags required to use together, if at all. Eg, `--foo` and `--bar` are optional, but if specified at all `--foo` and `--bar` must both have a value.
2. Additionally, `--baz` has a default value, in the same group of `--foo` and `--bar`.

### Actual Behavior Summary

The default value of `--baz` seems to make the `ArgGroup` behave as if the user is always attempting to use it. Such that the `ArgGroup` becomes required. Example:

```
> cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/test-clap-arggroup`
error: The following required arguments were not provided:

USAGE:
    test-clap-arggroup <--foo <FOO>|--bar <BAR>>

For more information try --help
```

### Steps to Reproduce the issue

Run the sample code, with no flags specified.

### Sample Code or Link to Sample Code

```
/// A bug report test binary
use clap::*;

fn main() {
  let _matches = App::new("bin")
    .arg(
      Arg::with_name("foo")
        .required(false)
        .long("foo")
        .value_name("FOO")
        .takes_value(true),
    )
    .arg(
      Arg::with_name("bar")
        .required(false)
        .long("bar")
        .value_name("BAR")
        .takes_value(true),
    )
    .arg(
      Arg::with_name("baz")
        .required(false)
        .long("baz")
        .value_name("BAZ")
        .default_value("default value")
        .takes_value(true),
    )
    .group(
      ArgGroup::with_name("test_flags")
        .multiple(true)
        .args(&["foo", "bar", "baz"])
        .requires_all(&["foo", "bar", "baz"]),
    )
    .get_matches();
}
```

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>

    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/test-clap-arggroup`
DEBUG:clap:Parser::propagate_settings: self=bin, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:foo: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:foo: doesn't have default vals
DEBUG:clap:Parser::add_defaults:iter:bar: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:bar: doesn't have default vals
DEBUG:clap:Parser::add_defaults:iter:baz: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:baz: has default vals
DEBUG:clap:Parser::add_defaults:iter:baz: wasn't used
DEBUG:clap:Parser::add_val_to_arg; arg=baz, val="default value"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."default value"
DEBUG:clap:Parser::groups_for_arg: name=baz
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
	Found 'test_flags'
DEBUG:clap:ArgMatcher::needs_more_vals: o=baz
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:baz;
DEBUG:clap:Parser::groups_for_arg: name=baz
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
	Found 'test_flags'
DEBUG:clap:Validator::validate_blacklist:iter:test_flags;
DEBUG:clap:Parser::groups_for_arg: name=test_flags
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
DEBUG:clap:Validator::validate_blacklist:iter:test_flags:group;
DEBUG:clap:Validator::validate_blacklist:iter:test_flags:group:iter:foo;
DEBUG:clap:Validator::validate_blacklist:iter:test_flags:group:iter:bar;
DEBUG:clap:Validator::validate_blacklist:iter:test_flags:group:iter:baz;
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:baz: vals=[
    "default value",
]
DEBUG:clap:Validator::validate_arg_num_vals:baz
DEBUG:clap:Validator::validate_arg_values: arg="baz"
DEBUG:clap:Validator::validate_arg_requires:baz;
DEBUG:clap:Validator::validate_arg_num_occurs: a=baz;
DEBUG:clap:Validator::validate_matched_args:iter:test_flags: vals=[
    "default value",
]
DEBUG:clap:Validator::missing_required_error: extra=None
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[]
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Validator::missing_required_error: req_args=""
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::smart_usage;
DEBUG:clap:usage::get_required_usage_from: reqs=["baz", "test_flags"], extra=None
DEBUG:clap:usage::get_required_usage_from:iter:test_flags: adding group req="foo"
DEBUG:clap:usage::get_required_usage_from:iter:test_flags: adding group req="bar"
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=["foo", "bar"]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["bar", "baz", "foo", "test_flags"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=["foo", "bar", "baz"]
DEBUG:clap:OptBuilder::fmt:foo
DEBUG:clap:OptBuilder::fmt:bar
DEBUG:clap:OptBuilder::fmt:baz
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::good;
error: The following required arguments were not provided:

USAGE:
    test-clap-arggroup <--foo <FOO>|--bar <BAR>|--baz <BAZ>>

For more information try --help

</code>
</pre>
</details>

---

_Renamed from "ArgGroup flag with a default, causes the entire ArgGroup to be required" to "requires_all ArgGroup with a default, causes the entire ArgGroup to be required" by @leeola on 2019-10-26 21:35_

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-02-01 11:27_

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 14:29_

---

_Label `C: arg groups` added by @CreepySkeleton on 2020-02-06 08:26_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-06 08:26_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-06 08:26_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 08:26_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-06 08:26_

---

_Unassigned @CreepySkeleton by @CreepySkeleton on 2020-02-06 08:29_

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 09:07_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:00_

---

_Comment by @LordMZTE on 2020-09-13 10:17_

any progress on this?

---

_Removed from milestone `3.1` by @pksunkara on 2020-09-13 18:04_

---

_Added to milestone `3.0` by @pksunkara on 2020-09-13 18:04_

---

_Label `W: 2.x` removed by @pksunkara on 2020-09-13 18:04_

---

_Label `W: 3.x` removed by @pksunkara on 2020-09-13 18:04_

---

_Assigned to @ldm0 by @ldm0 on 2021-03-13 13:15_

---

_Comment by @ldm0 on 2021-08-08 08:49_

I think this is the expected behaviour. Default value just behaves as if the user is always using it. If clap special case it, you will see an `ArgGroup` requires non present argument, but still successfully gets a value.

```rust
use clap::*;

fn main() {
  let _matches = App::new("bin")
    .arg(
      Arg::new("foo")
        .required(false)
        .long("foo")
        .value_name("FOO")
        .takes_value(true),
    )
    .arg(
      Arg::new("bar")
        .required(false)
        .long("bar")
        .value_name("BAR")
        .takes_value(true),
    )
    .arg(
      Arg::new("baz")
        .required(false)
        .long("baz")
        .value_name("BAZ")
        .default_value("default value")
        .takes_value(true),
    )
    .group(
      ArgGroup::new("test_flags")
        .multiple(true)
        .args(&["foo", "bar", "baz"])
        .requires_all(&["baz"]),
    )
    .get_matches();

    assert_eq!(_matches.value_of("test_flags"), Some("default value"));
}

```

---

_Closed by @ldm0 on 2021-08-08 08:49_

---

_Comment by @pksunkara on 2021-08-09 02:10_

I wouldn't consider this fixed if we need to change how default values interact with parsing. This is fully related to #1906. We had similar issues in #1605. Maybe we need to rethink this?

---

_Reopened by @pksunkara on 2021-08-09 02:10_

---

_Unassigned @ldm0 by @ldm0 on 2021-08-15 16:17_

---

_Referenced in [clap-rs/clap#2714](../../clap-rs/clap/issues/2714.md) on 2021-08-18 14:40_

---

_Referenced in [clap-rs/clap#2896](../../clap-rs/clap/pulls/2896.md) on 2021-10-16 19:24_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 19:37_

---

_Comment by @epage on 2021-10-16 19:38_

This is related to #2714 and I am reducing the impact through documentation in #2896. I assume @pksunkara will want this to remain open as part of future work to try to resolve this,

---

_Referenced in [clap-rs/clap#2897](../../clap-rs/clap/issues/2897.md) on 2021-10-16 19:48_

---

_Closed by @bors[bot] on 2021-10-27 21:31_

---

_Referenced in [epage/clapng#10](../../epage/clapng/issues/10.md) on 2021-11-17 21:58_

---

_Referenced in [clap-rs/clap#3076](../../clap-rs/clap/issues/3076.md) on 2021-12-07 21:56_

---
