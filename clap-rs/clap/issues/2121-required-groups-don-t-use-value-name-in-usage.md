```yaml
number: 2121
title: "Required groups don't use value_name in usage"
type: issue
state: closed
author: enterprisey
labels:
  - C-bug
assignees: []
created_at: 2020-08-31T05:57:30Z
updated_at: 2021-01-26T18:16:48Z
url: https://github.com/clap-rs/clap/issues/2121
synced_at: 2026-01-12T16:14:12Z
```

# Required groups don't use value_name in usage

---

_@enterprisey_

### Code

```rust
fn main() {
    clap::App::new("prog")
        .arg(clap::Arg::with_name("a").value_name("A"))
        .group(clap::ArgGroup::with_name("group").arg("a").required(true))
        .get_matches();
}
```

### Steps to reproduce the issue

 1. `cargo run -- --help`

### Version

* **Rust**: rustc 1.44.0 (49cae5576 2020-06-01)
* **Clap**: 2.33.3

### Actual Behavior Summary

`prog <a>` is printed for the usage string.

### Expected Behavior Summary

`prog <A>` is printed for the usage string.

### Additional context

See also #1292, perhaps?

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

DEBUG:clap:Parser::propagate_settings: self=x, g_settings=AppFlags(
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
DEBUG:clap:usage::get_required_usage_from: reqs=["group"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["group"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=["a"]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::get_args_tag;
DEBUG:clap:usage::get_args_tag:iter:a:
DEBUG:clap:Parser::groups_for_arg: name=a
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
	Found 'group'
DEBUG:clap:usage::get_args_tag:iter:a:iter:group;
DEBUG:clap:usage::create_help_usage: usage=x <a>
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
DEBUG:clap:Help::val: arg=&lt;A>
DEBUG:clap:Help::spec_vals: a=&lt;A>
DEBUG:clap:Help::val: Has switch...No, and not next_line
DEBUG:clap:write_spaces!: num=4
DEBUG:clap:Help::help;
DEBUG:clap:Help::help: Next Line...false
DEBUG:clap:Help::help: Too long...No
x

USAGE:
    x &lt;a>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    &lt;A>

</code>
</pre>
</details>


---

_Label `T: bug` added by @enterprisey on 2020-08-31 05:57_

---

_Label `C: usage strings` added by @pksunkara on 2020-09-01 07:50_

---

_Added to milestone `3.1` by @pksunkara on 2020-09-01 07:50_

---

_Referenced in [clap-rs/clap#2310](../../clap-rs/clap/pulls/2310.md) on 2021-01-24 19:04_

---

_Closed by @pksunkara on 2021-01-26 18:16_

---
