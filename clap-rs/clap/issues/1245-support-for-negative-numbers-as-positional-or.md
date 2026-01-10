---
number: 1245
title: Support for negative numbers as positional or named values.
type: issue
state: closed
author: ddrscott
labels: []
assignees: []
created_at: 2018-04-07T22:20:47Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1245
synced_at: 2026-01-10T01:26:46Z
---

# Support for negative numbers as positional or named values.

---

_Issue opened by @ddrscott on 2018-04-07 22:20_

I can't seem to find a way to elegantly use negative numbers as a positional argument or named argument.

I'm trying to make the CLI args something like:
```
calc add -123 345
```
but everything I've tried always returns:
```
error: Found argument '-1' which wasn't expected, or isn't valid in this context
```

### Rust Version

rustc 1.22.1 (05e2e1c41 2017-11-22)

### Affected Version of clap

2.31.2

### Expected Behavior Summary

Should distinguish negative numbers as number instead of flags.

### Actual Behavior Summary
```
error: Found argument '-1' which wasn't expected, or isn't valid in this context
```

### Steps to Reproduce the issue

```
cargo run -- -1.0
```

### Sample Code or Link to Sample Code
```
extern crate clap;
use clap::{Arg, App};

fn main() {
    let matches = App::new("Float Test")
        .arg(Arg::with_name("FLOAT")
             .help("positive or negative float value")
             .required(true)
             .index(1))
        .get_matches();
    println!("float: {}", matches.value_of("FLOAT").unwrap());
}
```

### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2", features = ["debug"] }
```
The output may be very long, so feel free to link to a gist or attach a text file

```
 ± % cargo run -- -1.0
   Compiling clap v2.31.2
   Compiling mandy v0.1.0 (file:///Users/scott.pierce/code/rust/mandy)
    Finished dev [unoptimized + debuginfo] target(s) in 7.39 secs
     Running `target/debug/mandy -1.0`
DEBUG:clap:Parser::propagate_settings: self=Float Test, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-1.0"' ([45, 49, 46, 48])
DEBUG:clap:Parser::is_new_arg:"-1.0":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-1.0"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-1.0"
DEBUG:clap:Parser::parse_short_arg:iter:1
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=["FLOAT"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["FLOAT"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=mandy <FLOAT>
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
error: Found argument '-1' which wasn't expected, or isn't valid in this context

USAGE:
    mandy <FLOAT>

For more information try --help
```

---

_Comment by @kbknapp on 2018-04-12 02:23_

This can be done on a per-argument basis using [`Arg::allow_hyphen_values(true)`](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.allow_hyphen_values) which simply allows an arguments value to start with a hyphen (`-`) or to *all* args in a command via [`AppSettings::AllowLeadingHyphen`](https://docs.rs/clap/2.31.2/clap/enum.AppSettings.html#variant.AllowLeadingHyphen). The latter requires greater can when designing the CLI as it can cause valid arguments to be treated as values in certain circumstances.

The technical details are clap uses the hyphen character to determine if a new argument is being parsed (assuming no other rules apply, such as number of values has been exhausted, etc.). Unfortunately this wraps up negative numbers. However, with the above settings you can opt-in to allowing negative numbers and relax clap's parsing rules.

---

_Closed by @kbknapp on 2018-04-12 02:23_

---

_Label `T: RFC / question` added by @kbknapp on 2018-04-12 02:23_

---

_Referenced in [yoshihitoh/ut-cli#3](../../yoshihitoh/ut-cli/issues/3.md) on 2019-06-21 15:44_

---

_Referenced in [jiricodes/ft_linear_regression#1](../../jiricodes/ft_linear_regression/issues/1.md) on 2022-01-06 09:21_

---

_Referenced in [typst/typst#970](../../typst/typst/pulls/970.md) on 2023-04-25 23:25_

---

_Referenced in [nextest-rs/nextest#1874](../../nextest-rs/nextest/pulls/1874.md) on 2024-11-15 11:02_

---

_Referenced in [pallets/click#2676](../../pallets/click/issues/2676.md) on 2024-11-28 23:22_

---
