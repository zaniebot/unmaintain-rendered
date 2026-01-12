```yaml
number: 1474
title: "`bin_name` overrides `name` in the help message"
type: issue
state: closed
author: sphynx
labels:
  - C-enhancement
  - A-help
  - ":money_with_wings: $5"
  - S-triage
assignees: []
created_at: 2019-05-20T17:47:48Z
updated_at: 2022-05-05T17:01:02Z
url: https://github.com/clap-rs/clap/issues/1474
synced_at: 2026-01-12T16:14:11Z
```

# `bin_name` overrides `name` in the help message

---

_@sphynx_

Maintainer's notes
- Fixing this might also fix #1431 and #1382
---
<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.36.0-nightly (af98304b9 2019-05-11)

### Affected Version of clap

2.33

### Bug or Feature Request Summary

If I use `bin_name` to override executable name in usage (for something like cargo command), `clap` also automatically overrides `name` (as well as argument to `App::new`) in the help message. It would be nice to have control over the name, since it appears at the very top of the help message.

### Expected Behavior Summary

```
> cargo run -- --help
appname

USAGE:
    bin name
```

### Actual Behavior Summary
```
> cargo run -- --help
bin-name

USAGE:
    bin name
```
### Steps to Reproduce the issue

`cargo run -- --help`

### Sample Code or Link to Sample Code

```rust
use clap::App;

fn main() {
    App::new("My Super Program")
        .name("appname")
        .bin_name("bin name")
        .get_matches();
}
```

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>

DEBUG:clap:Parser::propagate_settings: self=appname, g_settings=AppFlags(
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
DEBUG:clap:usage::create_help_usage: usage=bin name
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
bin-name

USAGE:
    bin name

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

</code>
</pre>
</details>


---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Label `T: enhancement` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Label `W: maybe` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Label `W: postponed` added by @CreepySkeleton on 2020-02-01 13:45_

---

_Comment by @Wtoll on 2020-09-08 03:48_

I second this being a necessary addition. I'm trying to write an extension to cargo (cargo-extension binary name as an example), and in order to have the usage information make sense, the bin name has to be set to "cargo extension", but then that forces the name in the version write out to be "cargo-extension" which is not very descriptive or helpful.

Considering cargo itself uses clap I think it makes sense for clap to be fully able to support writing extensions to cargo, and part of that includes allowing the "name" field to take precedence over the "bin-name" field to allow for both an accurate usage example, and a descriptive name.

Also, because I noticed the version number in this issue is different, this also equally affects clap v3.0.0-beta.1 which is what I've been developing with.

---

_Label `W: 2.x` removed by @pksunkara on 2020-10-05 15:29_

---

_Label `W: 3.x` removed by @pksunkara on 2020-10-05 15:29_

---

_Label `W: maybe` removed by @pksunkara on 2020-10-05 15:29_

---

_Label `W: postponed` removed by @pksunkara on 2020-10-05 15:29_

---

_Added to milestone `3.1` by @pksunkara on 2020-10-05 15:29_

---

_Comment by @pksunkara on 2021-05-26 03:26_

Note: Need to check if `NoBinaryName` solves it.

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-06-16 03:38_

---

_Referenced in [epage/clapng#119](../../epage/clapng/issues/119.md) on 2021-12-06 18:35_

---

_Label `S-triage` added by @epage on 2021-12-09 20:02_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 20:02_

---

_Comment by @epage on 2021-12-14 22:02_

From a comment of mine in #3096 

> From [help2man's docs](https://www.gnu.org/software/help2man/)
> 
> > Use argv[0] for the program name in these synopses, just as it is, with no directory stripping. This is in contrast to the canonical (constant) name of the program which is used in --version.
> 
> Interesting. They want the usage to use `argv[0]` but everywhere else to the use actual name.
> 
> For us, we show the bin name as the application name (see #992). Our bin name is `argv[0].file_name()` though with #992 I am considering changing it to `argv[0].file_stem()` or similar.



---

_Comment by @epage on 2022-05-04 21:14_

#3680 added a help_template variable for this.  I'm debating whether it can be the default in 3.2 (minor compatibility break) or 4.0 (major compatibility break). 

This is also helping #992

---

_Added to milestone `3.2` by @epage on 2022-05-04 21:14_

---

_Referenced in [clap-rs/clap#1431](../../clap-rs/clap/issues/1431.md) on 2022-05-04 21:38_

---

_Removed from milestone `3.2` by @epage on 2022-05-05 01:14_

---

_Added to milestone `4.0` by @epage on 2022-05-05 01:14_

---

_Comment by @epage on 2022-05-05 01:14_

We're close enough to when we can start working on 4.0, I'm going to play it safe and push it off to then

---

_Referenced in [clap-rs/clap#3693](../../clap-rs/clap/pulls/3693.md) on 2022-05-05 01:25_

---

_Closed by @epage on 2022-05-05 17:01_

---
