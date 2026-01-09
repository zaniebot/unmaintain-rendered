---
number: 1491
title: Parsing crashes with stack overflow if group name matches item name
type: issue
state: closed
author: Dentosal
labels:
  - C-bug
assignees: []
created_at: 2019-06-13T22:50:30Z
updated_at: 2020-04-10T02:00:15Z
url: https://github.com/clap-rs/clap/issues/1491
synced_at: 2026-01-07T13:12:19-06:00
---

# Parsing crashes with stack overflow if group name matches item name

---

_Issue opened by @Dentosal on 2019-06-13 22:50_

### Rust Version

`rustc 1.37.0-nightly (400b409ef 2019-06-09)` on macOS 10.14.4.

### Affected Version of clap

Both `2.33.0` and current master (784524f7eb193e35f81082cc69454c8c21b948f7).

### Bug Summary

Crashes with stack overflow if group name matches item name. At least I think that's the issue here.

### Expected Behavior Summary

Successful parse.

### Actual Behavior Summary

```
thread 'main' has overflowed its stack
fatal runtime error: stack overflow
Abort trap: 6
```

### Steps to Reproduce the issue

Create new binary crate, paste sample code to `src/main.rs` and run `cargo run -- -a`. Uses edition 2018 features.

### Sample Code or Link to Sample Code

```rust
fn main() {
    clap::App::new("name").arg(
        clap::Arg::with_name("a")
            .short("a") // the actual short flag doesn't matter
            .group("a"),
    ).get_matches();
}
```

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>
DEBUG:clap:Parser::propagate_settings: self=name, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-a"' ([45, 97])
DEBUG:clap:Parser::is_new_arg:"-a":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-a"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-a"
DEBUG:clap:Parser::parse_short_arg:iter:a
DEBUG:clap:Parser::parse_short_arg:iter:a: Found valid flag
DEBUG:clap:Parser::check_for_help_and_version_char;
DEBUG:clap:Parser::check_for_help_and_version_char: Checking if -a is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=a
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=a
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
	Found 'a'
DEBUG:clap:ArgMatcher::inc_occurrences_of: args=["a"]
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=a
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Flag
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("a");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:a;
DEBUG:clap:Parser::groups_for_arg: name=a
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
	Found 'a'
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:a: vals=[]
DEBUG:clap:Validator::validate_arg_requires:a;
DEBUG:clap:Validator::validate_arg_num_occurs: a=a;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::smart_usage;
DEBUG:clap:usage::get_required_usage_from: reqs=["a"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["a"]

thread 'main' has overflowed its stack
fatal runtime error: stack overflow
Abort trap: 6
</code>
</pre>
</details>


---

_Comment by @jcdyer on 2019-06-17 14:38_

I would not expect a successful parse for this.  Groups can be used in the same context as Args, so successfully parsing this would lead to ambiguous behavior when calling (for example) `matches.is_present("a")`.  I agree that a stack overflow here is a bug, but I think a better solution would be to fail with a panic, as is already done for duplicate arguments.

```
thread 'main' panicked at 'Non-unique argument name: a is already in use', ~/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.0/src/app/parser.rs:174:9
```

The error message would need to be a little different, of course.


---

_Referenced in [TeXitoi/structopt#203](../../TeXitoi/structopt/issues/203.md) on 2019-06-17 21:55_

---

_Added to milestone `v3.0.0-alpha.3` by @CreepySkeleton on 2020-02-01 13:26_

---

_Label `C: args` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `C: options` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `P1: urgent` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 13:28_

---

_Removed from milestone `3.0.0-beta.2` by @pksunkara on 2020-04-07 13:59_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-07 13:59_

---

_Label `C: asserts` added by @pksunkara on 2020-04-09 07:58_

---

_Label `W: 2.x` removed by @pksunkara on 2020-04-09 07:58_

---

_Label `W: 3.x` removed by @pksunkara on 2020-04-09 07:58_

---

_Label `help wanted` removed by @pksunkara on 2020-04-09 07:58_

---

_Closed by @bors[bot] on 2020-04-10 02:00_

---

_Referenced in [TeXitoi/structopt#384](../../TeXitoi/structopt/issues/384.md) on 2020-05-01 17:16_

---
