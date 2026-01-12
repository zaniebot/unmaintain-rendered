```yaml
number: 3024
title: "Incorrect parsing - The subcommand 'bar' wasn't recognized, did you mean 'bar'?"
type: issue
state: closed
author: Ten0
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2021-11-14T13:04:38Z
updated_at: 2021-11-16T16:23:10Z
url: https://github.com/clap-rs/clap/issues/3024
synced_at: 2026-01-12T16:14:14Z
```

# Incorrect parsing - The subcommand 'bar' wasn't recognized, did you mean 'bar'?

---

_@Ten0_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.1 (59eed8a2a 2021-11-01)

### Clap Version

2.33.3

### Minimal reproducible code

```rust
use clap::*;
fn main() {
	let m = App::new("prog")
		.arg(Arg::with_name("foo").long("foo").multiple(true).value_terminator("--"))
		.subcommand(SubCommand::with_name("bar"))
		.get_matches_from(vec!["prog", "--foo", "1", "2", "--", "bar"]);
	let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
	assert_eq!(&cmds, &["1", "2"]);
	assert_eq!(m.subcommand().0, "bar");
}
```

### Steps to reproduce the bug with the above code

```bash
cargo run
```

### Actual Behaviour

```
error: The subcommand 'bar' wasn't recognized
	Did you mean 'bar'?
```

### Expected Behaviour

*Runs without panicking*

### Additional Context

This is always an invalid error message regardless of the context in which it happens: it knows it's trying to parse a subcommand, it reads it properly, but doesn't match it to the exact same existing subcommand.

The same thing happens regardless of my value terminator (even if it's not `--`). I would expect `--` in the args to still be treated at least as a terminator regardless, especially considering I didn't set `allow_hyphen_values`.

However, `multiple` won't accept multiple values in a single occurrence and instead be treated as a flag if I don't specify an arbitrary value terminator, despite this being what the doc says it would do (https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.multiple):

```rust
use clap::*;
fn main() {
	let m = App::new("prog")
		.arg(Arg::with_name("foo").long("foo"))
		.get_matches_from(vec!["prog", "--foo", "1", "2"]);
	let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
	assert_eq!(&cmds, &["1", "2"]);
}
```
gives:
```
error: Found argument '1' which wasn't expected, or isn't valid in this context

USAGE:
    prog --foo

For more information try --help
```
and `--help` gives
```
USAGE:
    prog [FLAGS]

FLAGS:
        --foo        
    -h, --help       Prints help information
    -V, --version    Prints version information
```

Is this a separate issue? Was it already filed? Related to #2599 maybe?

### Debug Output

```
DEBUG:clap:Parser::add_subcommand: term_w=None, name=bar
DEBUG:clap:Parser::propagate_settings: self=prog, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=bar, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=bar, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--foo"' ([45, 45, 102, 111, 111])
DEBUG:clap:Parser::is_new_arg:"--foo":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--foo"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:OptBuilder::fmt:foo
DEBUG:clap:Parser::parse_long_arg: Found valid opt '--foo <foo>...'
DEBUG:clap:Parser::parse_opt; opt=foo, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE | EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=foo
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:Parser:get_matches_with: After parse_long_arg Opt("foo")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"1"' ([49])
DEBUG:clap:Parser::is_new_arg:"1":Opt("foo")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=foo, val="1"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."1"
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=foo
DEBUG:clap:Parser::get_matches_with: Begin parsing '"2"' ([50])
DEBUG:clap:Parser::is_new_arg:"2":Opt("foo")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=foo, val="2"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."2"
DEBUG:clap:Parser::groups_for_arg: name=foo
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=foo
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--"' ([45, 45])
DEBUG:clap:Parser::is_new_arg:"--":Opt("foo")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::get_matches_with: setting TrailingVals=true
DEBUG:clap:Parser::get_matches_with: Begin parsing '"bar"' ([98, 97, 114])
DEBUG:clap:Parser::is_new_arg:"bar":Opt("foo")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::smart_usage;
DEBUG:clap:usage::get_required_usage_from: reqs=["foo"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["foo"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:foo:
DEBUG:clap:OptBuilder::fmt:foo
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;
error: The subcommand 'bar' wasn't recognized
	Did you mean 'bar'?

If you believe you received this message in error, try re-running with 'prog -- bar'

USAGE:
    prog --foo <foo>...

For more information try --help
```

---

_Label `T: bug` added by @Ten0 on 2021-11-14 13:04_

---

_Referenced in [TeXitoi/structopt#509](../../TeXitoi/structopt/issues/509.md) on 2021-11-14 13:11_

---

_Comment by @epage on 2021-11-15 16:16_

Gave this a try on master (clap3)
```rust
use clap::*;
fn main() {
    let m = App::new("prog")
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .multiple_values(true)
                .value_terminator("--"),
        )
        .subcommand(SubCommand::with_name("bar"))
        .get_matches_from(vec!["prog", "--foo", "1", "2", "--", "bar"]);
    let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
    assert_eq!(&cmds, &["1", "2"]);
    assert_eq!(m.subcommand().unwrap().0, "bar");
}
```
gave
```
error: Found argument 'bar' which wasn't expected, or isn't valid in this context

        If you tried to supply `bar` as a subcommand, remove the '--' before it.

USAGE:
    prog [OPTIONS] [SUBCOMMAND]

For more information try --help
```

If I switch the value terminator, the command runs successfully.
```rust
use clap::*;
fn main() {
    let m = App::new("prog")
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .multiple_values(true)
                .value_terminator(";"),
        )
        .subcommand(SubCommand::with_name("bar"))
        .get_matches_from(vec!["prog", "--foo", "1", "2", ";", "bar"]);
    let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
    assert_eq!(&cmds, &["1", "2"]);
    assert_eq!(m.subcommand().unwrap().0, "bar");
}
```

Within clap3, my guess is there is a precedence issue between clap's built-in `--` processing and a `--` value terminator.

---

_Label `C: parsing` added by @epage on 2021-11-15 16:17_

---

_Comment by @epage on 2021-11-15 16:19_

> The same thing happens regardless of my value terminator (even if it's not --)

Could you provide an example?  I just tried this in clap2 and it worked for me
```rust
use clap::*;
fn main() {
    let m = App::new("prog")
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .multiple(true)
                .value_terminator(";"),
        )
        .subcommand(SubCommand::with_name("bar"))
        .get_matches_from(vec!["prog", "--foo", "1", "2", ";", "bar"]);
    let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
    assert_eq!(&cmds, &["1", "2"]);
    assert_eq!(m.subcommand().0, "bar");
}
```

---

_Comment by @epage on 2021-11-15 16:25_

> However, multiple won't accept multiple values in a single occurrence and instead be treated as a flag if I don't specify an arbitrary value terminator, despite this being what the doc says it would do (https://docs.rs/clap/2.33.3/clap/struct.Arg.html#method.multiple):

Thats because its a flag by default.  If you look at the `multiple` examples, they are multiple flags unless you pass `takes_value(true)`.  That gives you
```rust
use clap::*;
fn main() {
    let m = App::new("prog")
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .multiple(true)
                .takes_value(true),
        )
        .subcommand(SubCommand::with_name("bar"))
        .get_matches_from(vec!["prog", "--foo", "1", "2", ";", "bar"]);
    let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
    assert_eq!(&cmds, &["1", "2"]);
    assert_eq!(m.subcommand().0, "bar");
}
```
which fails with 
```
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `["1", "2", ";", "bar"]`,
 right: `["1", "2"]`', src/main.rs:42:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

However, in clap3, we added a new `AppSettings::SubcommandPrecedenceOverArg` to help in this situation which makes it pass without a value terminator
```rust
use clap::*;
fn main() {
    let m = App::new("prog")
        .setting(AppSettings::SubcommandPrecedenceOverArg)
        .arg(
            Arg::with_name("foo")
                .long("foo")
                .multiple(true)
                .takes_value(true),
        )
        .subcommand(SubCommand::with_name("bar"))
        .get_matches_from(vec!["prog", "--foo", "1", "2", "bar"]);
    let cmds: Vec<_> = m.values_of("foo").unwrap().collect();
    assert_eq!(&cmds, &["1", "2"]);
    assert_eq!(m.subcommand().unwrap().0, "bar");
}
```

---

_Comment by @pksunkara on 2021-11-15 17:20_

Is there anything actionable here that I am missing or can this be closed?

---

_Comment by @epage on 2021-11-15 17:25_

There is one question to Ten0 to get more information on one of the reported problems ("regardless of value terminator").

I'm also interested from any parties on our level of concern over:

> Within clap3, my guess is there is a precedence issue between clap's built-in -- processing and a -- value terminator.

---

_Comment by @Ten0 on 2021-11-16 16:23_

> Could you provide an example? I just tried this in clap2 and it worked for me

Sorry I was unclear, I meant if I change the value terminator, but not the input.

I understand this will be fixed by clap 3 -> Closing. Thanks! :)

---

_Closed by @Ten0 on 2021-11-16 16:23_

---
