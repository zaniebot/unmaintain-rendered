```yaml
number: 909
title: "USAGE change when not using \"=\" before argument"
type: issue
state: closed
author: logaritmisk
labels: []
assignees: []
created_at: 2017-03-21T09:37:42Z
updated_at: 2018-08-02T03:30:04Z
url: https://github.com/clap-rs/clap/issues/909
synced_at: 2026-01-12T16:14:10Z
```

# USAGE change when not using "=" before argument

---

_@logaritmisk_

### Rust Version
rustc 1.16.0 (30cf806ef 2017-03-10)

### Affected Version of clap
0d2a3f6

### Expected Behavior Summary
Using`-l swe` to be the same as `-l=swe`

### Actual Behavior Summary
Usage change when using `-l swe`.

### Steps to Reproduce the issue
```shell
$ cargo run -- --help

Test

USAGE:
    tester [OPTIONS] <PATH>...

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
    -l <LANGUAGE>...

ARGS:
    <PATH>...
```
**This do not work**
```shell
$ cargo run -- -l swe target

error: The following required arguments were not provided:
    <PATH>...

USAGE:
    tester <PATH>... -l <LANGUAGE>...

For more information try --help
```

**But this works**
```shell
$ cargo run -- -l=swe target

ArgMatches {
    args: {
        "PATH": MatchedArg {
            occurs: 1,
            vals: [
                "target"
            ]
        },
        "language": MatchedArg {
            occurs: 1,
            vals: [
                "swe"
            ]
        }
    },
    subcommand: None,
    usage: Some(
        "USAGE:\n    tester [OPTIONS] <PATH>..."
    )
}
```

### Sample Code or Link to Sample Code
```rust
extern crate clap;

use clap::{App, Arg};

fn main() {
    let matches = App::new("Test")
        .arg(Arg::with_name("language")
             .short("l")
             .value_name("LANGUAGE")
             .multiple(true))
        .arg(Arg::with_name("PATH")
             .multiple(true)
             .required(true))
        .get_matches();

    println!("{:#?}", matches);
}
```

### Debug output
```
DEBUG:clap:Parser::propogate_settings: self=Test, g_settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-l"' ([45, 108])
DEBUG:clap:Parser::is_new_arg: arg="-l", Needs Val of=None
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-l"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-l"
DEBUG:clap:Parser::parse_short_arg:iter:l
DEBUG:clap:Parser::parse_short_arg:iter:l: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:l: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=language, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(MULTIPLE | EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=language
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=language
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:arg_post_processing!;
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Is '-l <LANGUAGE>...' in overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '-l <LANGUAGE>...' have overrides...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '-l <LANGUAGE>...' have conflicts...No
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:arg_post_processing!: Does '-l <LANGUAGE>...' have requirements...No
DEBUG:clap:_handle_group_reqs!;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"swe"' ([115, 119, 101])
DEBUG:clap:Parser::is_new_arg: arg="swe", Needs Val of=Some("language")
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="swe"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::add_val_to_arg; arg=language, val="swe"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."swe"
DEBUG:clap:Parser::groups_for_arg: name=language
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=language
DEBUG:clap:Parser::get_matches_with: Begin parsing '"target"' ([116, 97, 114, 103, 101, 116])
DEBUG:clap:Parser::is_new_arg: arg="target", Needs Val of=Some("language")
DEBUG:clap:Parser::is_new_arg: Arg::allow_leading_hyphen(false)
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="target"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::add_val_to_arg; arg=language, val="target"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."target"
DEBUG:clap:Parser::groups_for_arg: name=language
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=language
DEBUG:clap:Validator::validate;
DEBUG:clap:Validator::validate: needs_val_of="language"
DEBUG:clap:Validator::validate_required: required=["PATH"];
DEBUG:clap:Validator::validate_required:iter:PATH:
DEBUG:clap:Validator::is_missing_required_ok: a=PATH
DEBUG:clap:Validator::validate_conflicts: a="PATH";
DEBUG:clap:Validator::validate_required_unless: a="PATH";
DEBUG:clap:Validator::missing_required_error: extra=None
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:Validator::missing_required_error: reqs=[
    "PATH"
]
DEBUG:clap:usage::get_required_usage_from: reqs=["PATH"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["PATH"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Colorizer::error;
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: req_args="\n    \u{1b}[1;31m<PATH>...\u{1b}[0m"
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::smart_usage;
DEBUG:clap:usage::get_required_usage_from: reqs=["PATH", "language"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["PATH", "language"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:language:
DEBUG:clap:OptBuilder::fmt
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:Colorizer::error;
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::good;
DEBUG:clap:is_a_tty: stderr=true
```

---

_Comment by @logaritmisk on 2017-03-21 10:33_

Got it working by using `.number_of_values(1)`. But still, think it is strange that it works with `-l=swe` and not `-l swe`.

---

_Comment by @kbknapp on 2017-03-22 00:36_

This is because there's no way for clap to know when to stop parsing `--language` because it's been told it accepts multiple values, without providing some additional information (such as `number_of_values`). clap looks for a leading hyphen, in the absence of additonal information, to know when to start parsing a new arg.

This would work `$ cargo run -- target --language swe`

The other thing you could do (if `number_of_values(1)` isn't to your liking) is specify `require_equals(true)` which will give an error if `--language swe` is used.

---

_Label `T: RFC / question` added by @kbknapp on 2017-03-22 00:36_

---

_Closed by @kbknapp on 2017-04-05 00:50_

---
