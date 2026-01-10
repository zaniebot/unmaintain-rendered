---
number: 1622
title: Conflicting Flags Error message contains the same flag twice
type: issue
state: closed
author: lucidBrot
labels:
  - C-bug
  - A-help
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2020-01-04T21:57:45Z
updated_at: 2020-03-08T20:10:48Z
url: https://github.com/clap-rs/clap/issues/1622
synced_at: 2026-01-10T01:27:00Z
---

# Conflicting Flags Error message contains the same flag twice

---

_Issue opened by @lucidBrot on 2020-01-04 21:57_

### Rust Version
```
$ rustc -V
rustc 1.39.0 (4560ea788 2019-11-04)
```

### Affected Version of clap

```
$ grep clap debugcli/Cargo.toml
clap = { git = "https://github.com/clap-rs/clap.git", rev = "1c3958c", features = ["color", "suggestions", "derive"] }

$ grep clap debugcli/Cargo.lock
name = "clap"
source = "git+https://github.com/clap-rs/clap.git?rev=1c3958c#1c3958c56eca35235ed6837c666adb8ede979961"
 "clap_derive 0.3.0 (git+https://github.com/clap-rs/clap_derive)",
name = "clap_derive"
source = "git+https://github.com/clap-rs/clap_derive#2622165be3f061f65fe6b345f4235e7365918244"
 "clap 3.0.0-beta.1 (git+https://github.com/clap-rs/clap.git?rev=1c3958c)",
"checksum clap 3.0.0-beta.1 (git+https://github.com/clap-rs/clap.git?rev=1c3958c)" = "<none>"
"checksum clap_derive 0.3.0 (git+https://github.com/clap-rs/clap_derive)" = "<none>"
```

### Expected Behavior Summary
I have two conflicting flags `-d` and `-D`. When the user passes `-dD` I expect the output to contain
```
error: The argument '-d' cannot be used with '-D'
```
### Actual Behavior Summary
The output instead contains
```
error: The argument '-d' cannot be used with '-d'
```

Same happens if the flags have a long name as well.

### Steps to Reproduce the issue
`cargo new --bin debugcli`  

Set up Cargo.toml as follows:
```
[package]
name = "debugcli"
version = "0.1.0"
authors = ["Eric Mink <eric@mink.li>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = { git = "https://github.com/clap-rs/clap.git", rev = "1c3958c", features = ["color", "suggestions", "derive"] }

[workspace]
```

Set up `src/main.rs` as follows:
```
extern crate clap;
use clap::*;

#[derive(Clap)]
struct Opts {
    /// Print debug info
    #[clap(short = "d", conflicts_with = "not_debug")]
    debug: bool,
    /// Explicitly disable debug printing
    #[clap(short = "D")]
    not_debug: bool,

}

fn main() {
    let t: Opts = Opts::parse();

    // You can handle information about subcommands by requesting their matches by name
    // (as below), requesting just the name used, or both at the same time
            if t.debug { println!("testing with debug output on."); }
            if t.not_debug { println!("testing with debug output off."); }
            if t.debug && t.not_debug { println!("WTF!"); }
            if !t.debug && !t.not_debug { println!("Defaulting to testing with debug output off"); }
    }
```

Run it with various combinations:
```
$ cargo run -- -h
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\debugcli.exe -h`
debugcli 0.1.0
Eric Mink <eric@mink.li>

USAGE:
    debugcli.exe [FLAGS]

FLAGS:
    -d               Print debug info
    -h, --help       Prints help information
    -D               Explicitly disable debug printing
    -V, --version    Prints version information

$ cargo run --
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target\debug\debugcli.exe`
Defaulting to testing with debug output off
$ cargo run -- -D
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\debugcli.exe -D`
testing with debug output off.
$ cargo run -- -d
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\debugcli.exe -d`
testing with debug output on.
$ cargo run -- -dD
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\debugcli.exe -dD`
error: The argument '-d' cannot be used with '-d'

USAGE:
    debugcli.exe [FLAGS]

For more information try --help
error: process didn't exit successfully: `target\debug\debugcli.exe -dD` (exit code: 1)
$ cargo run -- -Dd
    Finished dev [unoptimized + debuginfo] target(s) in 0.06s
     Running `target\debug\debugcli.exe -Dd`
error: The argument '-D' cannot be used with '-D'

USAGE:
    debugcli.exe [FLAGS]

For more information try --help
error: process didn't exit successfully: `target\debug\debugcli.exe -Dd` (exit code: 1)

```

It is worth noting that in above pasted output, it is once `The argument '-D' cannot be used with '-D'` and once `The argument '-d' cannot be used with '-d'`. But when you use the program _exactly_ as I above, it will always be `The argument '-d' cannot be used with '-d'`.  
The program that produced the above output had one difference to the program I pasted here: It had a `conflicts_with = "debug"` annotation for the `D` flag.


### Debug output
<details>
<summary> Debug Output </summary>
<pre>
<code>

$ cargo run -- -dD
   Compiling clap v3.0.0-beta.1 (https://github.com/clap-rs/clap.git?rev=1c3958c#1c3958c5)
   Compiling debugcli v0.1.0 (C:\Users\eric\Documents\projects\cevi-versand\debugcli)
    Finished dev [unoptimized + debuginfo] target(s) in 14.49s
     Running `target\debug\debugcli.exe -dD`
DEBUG:clap:App::_do_parse;
DEBUG:clap:App::_build;
DEBUG:clap:App::_derive_display_order:debugcli
DEBUG:clap:App::_create_help_and_version;
DEBUG:clap:App::_create_help_and_version: Building --help
DEBUG:clap:App::_create_help_and_version: Building --version
DEBUG:clap:App::_arg_debug_asserts:debug
DEBUG:clap:App::_arg_debug_asserts:not_debug
DEBUG:clap:App::_arg_debug_asserts:help
DEBUG:clap:App::_arg_debug_asserts:version
DEBUG:clap:App::_app_debug_asserts;
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::_build;
DEBUG:clap:Parser::_verify_positionals;
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-dD"' ([45, 100, 68])
DEBUG:clap:Parser::is_new_arg:"-dD":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-dD"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:Parser::parse_short_arg: full_arg="-dD"
DEBUG:clap:Parser::parse_short_arg:iter:d
DEBUG:clap:Parser::parse_short_arg:iter:d: Found valid opt or flag
DEBUG:clap:Parser::check_for_help_and_version_char;
DEBUG:clap:Parser::check_for_help_and_version_char: Checking if -d is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=353070537278882053
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=353070537278882053
DEBUG:clap:Parser::parse_short_arg:iter:D
DEBUG:clap:Parser::parse_short_arg:iter:D: Found valid opt or flag
DEBUG:clap:Parser::check_for_help_and_version_char;
DEBUG:clap:Parser::check_for_help_and_version_char: Checking if -D is help or version...Neither
DEBUG:clap:Parser::parse_flag;
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=15241144975642668291
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=15241144975642668291
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Flag
DEBUG:clap:Parser::remove_overrides;
DEBUG:clap:Parser::remove_overrides:iter:353070537278882053;
DEBUG:clap:Parser::remove_overrides:iter:15241144975642668291;
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Validator::validate_conflicts;
DEBUG:clap:Validator::gather_conflicts;
DEBUG:clap:Validator::gather_conflicts:iter:353070537278882053;
DEBUG:clap:Parser::groups_for_arg: name=353070537278882053
DEBUG:clap:Validator::gather_conflicts:iter:15241144975642668291;
DEBUG:clap:Parser::groups_for_arg: name=15241144975642668291
DEBUG:clap:Validator::validate_conflicts:iter:15241144975642668291;
DEBUG:clap:Validator::validate_conflicts:iter:15241144975642668291: matcher contains it...
DEBUG:clap:build_err!: name=15241144975642668291
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:Usage::create_help_usage; incl_reqs=true
DEBUG:clap:Usage::get_required_usage_from: incls=[], matcher=false, incl_last=false
DEBUG:clap:Usage::get_required_usage_from: ret_val=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=debug;
DEBUG:clap:Parser::groups_for_arg: name=353070537278882053
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=debugcli.exe [FLAGS]
DEBUG:clap:App::color;
DEBUG:clap:App::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::good;
error: The argument '-d' cannot be used with '-d'

USAGE:
    debugcli.exe [FLAGS]

For more information try --help
error: process didn't exit successfully: `target\debug\debugcli.exe -dD` (exit code: 1)

</code>
</pre>
</details>


Please excuse if this has been already mentioned, I did not find any open issue for it.

---

_Referenced in [lucidBrot/cevi-versand#35](../../lucidBrot/cevi-versand/issues/35.md) on 2020-01-04 21:58_

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-02-01 10:48_

---

_Label `C: errors` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Label `good first issue` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Label `help wanted` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Label `T: bug` added by @CreepySkeleton on 2020-02-01 10:49_

---

_Referenced in [tealdeer-rs/tealdeer#108](../../tealdeer-rs/tealdeer/pulls/108.md) on 2020-03-07 18:32_

---

_Comment by @sphinxc0re on 2020-03-08 14:01_

I believe these are the affected lines (180 is the culprit): https://github.com/clap-rs/clap/blob/e4a7f5012855e9dd906caaa7028393cf1926da8e/src/parse/validator.rs#L177-L184

---

_Comment by @pksunkara on 2020-03-08 14:03_

Will you be sending a PR? I can fix this tonight if not.

---

_Referenced in [clap-rs/clap#1732](../../clap-rs/clap/pulls/1732.md) on 2020-03-08 15:39_

---

_Closed by @bors[bot] on 2020-03-08 20:10_

---

_Closed by @bors[bot] on 2020-03-08 20:10_

---
