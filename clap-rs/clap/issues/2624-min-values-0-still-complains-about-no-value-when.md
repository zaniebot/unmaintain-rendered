---
number: 2624
title: "`min_values(0)` still complains about no value when combining short versions"
type: issue
state: closed
author: miDeb
labels:
  - C-bug
assignees: []
created_at: 2021-07-26T14:55:38Z
updated_at: 2021-07-30T09:37:41Z
url: https://github.com/clap-rs/clap/issues/2624
synced_at: 2026-01-10T01:27:22Z
---

# `min_values(0)` still complains about no value when combining short versions

---

_Issue opened by @miDeb on 2021-07-26 14:55_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0-nightly (9c25eb7aa 2021-07-25)

### Clap Version

2.33

### Minimal reproducible code

```rust
use clap::{App, Arg};

fn main() {
    let matches = App::new("foo")
        .arg(
            Arg::with_name("check")
                .short("c")
                .long("check")
                .takes_value(true)
                .require_equals(true)
                .min_values(0)
                .possible_values(&["silent", "quiet", "diagnose-first"])
                .help("check for sorted input; do not sort"),
        )
        .arg(
            Arg::with_name("unique")
                .short("u")
                .long("unique")
                .help("output only the first of an equal run"),
        )
        .get_matches();
    assert!(matches.is_present("check"));
    assert!(matches.is_present("unique"));
}

```


### Steps to reproduce the bug with the above code

cargo r -- -cu

### Actual Behaviour

```plaintext
error: The argument '--check=<check>' requires a value but none was supplied

USAGE:
    clap-bug [FLAGS] [OPTIONS]

For more information try --help
```

### Expected Behaviour

The same thing that happens when invoking the program like this: `cargo r -- -u -c`, which should be synomymous to `-cu`.
The program would succeed and produce no output.

### Additional Context

_No response_

### Debug Output

```plaintext
DEBUG:clap:Parser::propagate_settings: self=foo, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-cu"' ([45, 99, 117])
DEBUG:clap:Parser::is_new_arg:"-cu":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-cu"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-cu"
DEBUG:clap:Parser::parse_short_arg:iter:c
DEBUG:clap:Parser::parse_short_arg:iter:c: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:c: p[0]=[], p[1]=[117]
DEBUG:clap:Parser::parse_short_arg:iter:c: val=[117] (bytes), val="u" (ascii)
DEBUG:clap:Parser::parse_opt; opt=check, val=Some("u")
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(TAKES_VAL | DELIM_NOT_SET | REQUIRE_EQUALS)
DEBUG:clap:Parser::parse_opt; Checking for val...Found Empty - Error
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=unique;
DEBUG:clap:Parser::groups_for_arg: name=unique
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=clap-bug [FLAGS] [OPTIONS]
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:OptBuilder::fmt:check
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
error: The argument '--check=<check>' requires a value but none was supplied

USAGE:
    clap-bug [FLAGS] [OPTIONS]

For more information try --help
```

---

_Label `T: bug` added by @miDeb on 2021-07-26 14:55_

---

_Comment by @epage on 2021-07-26 15:31_

Looks to be an ordering issue.  We pull out the value before checking `RequireEquals`

I feel like this is something that would be made easier to fix once we've refactored to having a dedicated lexer, see https://github.com/clap-rs/clap/discussions/2615

---

_Referenced in [uutils/coreutils#2520](../../uutils/coreutils/pulls/2520.md) on 2021-07-26 16:04_

---

_Comment by @ldm0 on 2021-07-26 16:07_

Current master behaviour:
```rust
use clap::{App, Arg};
fn main() {
    let matches = App::new("foo")
        .arg(
            Arg::new("check")
                .short('c')
                .long("check")
                .require_equals(true)
                .min_values(0)
                .possible_values(&["silent", "quiet", "diagnose-first"]),
        )
        .arg(
            Arg::new("unique")
                .short('u')
                .long("unique")
        )
        .get_matches();
    assert!(matches.is_present("check"));
    assert!(matches.is_present("unique"));
}

```

```rust
error: "u" isn't a valid value for '--check=<check>...'
        [possible values: diagnose-first, quiet, silent]

USAGE:
    rust_test2 --check=<check>...

For more information try --help
```


---

_Comment by @miDeb on 2021-07-26 16:08_

This looks like it doesn't take `require_equals(true)` into account on current master.

---

_Referenced in [clap-rs/clap#2643](../../clap-rs/clap/pulls/2643.md) on 2021-07-30 04:07_

---

_Closed by @pksunkara on 2021-07-30 09:37_

---
