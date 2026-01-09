---
number: 1382
title: the --version flag concatenates subcommands
type: issue
state: open
author: Geal
labels:
  - C-bug
  - A-help
  - S-blocked
assignees: []
created_at: 2018-11-15T09:05:26Z
updated_at: 2022-02-03T14:55:44Z
url: https://github.com/clap-rs/clap/issues/1382
synced_at: 2026-01-07T13:12:19-06:00
---

# the --version flag concatenates subcommands

---

_Issue opened by @Geal on 2018-11-15 09:05_

Maintainer's notes
- We hope #1334 will provide a way to resolve this.  This is being left open to ensure this use case isn't lost in case a minimum or full solution doesn't include this
---
### Rust Version

`rustc 1.31.0-nightly (4bd4e4130 2018-10-25)`

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

Passing the `--version` flag after one or more subcommands should not append the subcommand names to the process name

### Expected Behavior Summary

```
$ ./target/debug/clap-test --version
claptest 1.0
$ ./target/debug/clap-test test --version
claptest 1.0
$ ./target/debug/clap-test test hello --version
claptest 1.0
```

### Actual Behavior Summary

```
$ ./target/debug/clap-test --version
claptest 1.0
$ ./target/debug/clap-test test --version
clap-test-test 
$ ./target/debug/clap-test test hello --version
clap-test-test-hello
```

### Sample Code or Link to Sample Code

```rust
extern crate clap;
use clap::{Arg, App, SubCommand};

fn main() {
	let matches = App::new("claptest")
		.version("1.0")
		.arg(Arg::with_name("config")
				 .short("c")
				 .long("config")
				 .value_name("FILE")
				 .help("Sets a custom config file")
				 .takes_value(true))
		.subcommand(SubCommand::with_name("test")
								.help("print debug information verbosely")
								.subcommand(SubCommand::with_name("hello")
														.help("hello")))
		.get_matches();
}
```

### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2", features = ["debug"] }
```
Output for: `./target/debug/clap-test test --version`:

<details>
<summary> Debug Output </summary>
<pre>
<code>

DEBUG:clap:Parser::add_subcommand: term_w=None, name=hello
DEBUG:clap:Parser::add_subcommand: term_w=None, name=test
DEBUG:clap:Parser::propagate_settings: self=claptest, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=test, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=test, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: sc=hello, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
), g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::propagate_settings: self=hello, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"test"' ([116, 101, 115, 116])
DEBUG:clap:Parser::is_new_arg:"test":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="test"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("test")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=test
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"--version"' ([45, 45, 118, 101, 114, 115, 105, 111, 110])
DEBUG:clap:Parser::is_new_arg:"--version":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: -- found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="--version"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_long_arg;
DEBUG:clap:Parser::parse_long_arg: Does it contain '='...No
DEBUG:clap:Parser::parse_long_arg: Found valid flag '--version'
DEBUG:clap:Parser::check_for_help_and_version_str;
DEBUG:clap:Parser::check_for_help_and_version_str: Checking if --version is help or version...Version
DEBUG:clap:Parser::_version: 
clap-test-test 


</code>
</pre>
</details>

this does not look like a critical bug, but it still feels weird :)

---

_Comment by @EverlastingBugstopper on 2019-10-28 22:19_

+1 on this. Thought we had a bug in our implementation but it seems to be the way clap handles versions for subcommands. It can be overridden with a `.version` on each subcommand, but my feeling is that it should default to the parent's version. https://github.com/cloudflare/wrangler/issues/791

---

_Referenced in [cloudflare/wrangler-legacy#791](../../cloudflare/wrangler-legacy/issues/791.md) on 2019-10-28 22:19_

---

_Label `C: help message` added by @CreepySkeleton on 2020-02-01 15:31_

---

_Label `C: subcommands` added by @CreepySkeleton on 2020-02-01 15:31_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:41_

---

_Referenced in [clap-rs/clap#1431](../../clap-rs/clap/issues/1431.md) on 2021-02-22 09:30_

---

_Referenced in [clap-rs/clap#2564](../../clap-rs/clap/issues/2564.md) on 2021-06-22 14:00_

---

_Referenced in [epage/clapng#108](../../epage/clapng/issues/108.md) on 2021-12-06 17:39_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:11_

---

_Removed from milestone `3.1` by @epage on 2021-12-08 20:11_

---

_Comment by @epage on 2021-12-08 20:12_

This seems like a slight variant on https://github.com/clap-rs/clap/issues/1431.  We should probably look at these together.

---

_Label `C-bug` added by @epage on 2021-12-09 19:12_

---

_Comment by @epage on 2021-12-09 19:13_

One other aspect of this is that we can have per-subcommand versions.  In that case, we probably would want to list what the version is relevant for.

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 19:13_

---

_Comment by @epage on 2022-02-03 14:51_

Updated the code for clap3
```rust
use clap::{App, AppSettings, Arg};

fn main() {
    let matches = App::new("claptest")
        .setting(AppSettings::PropagateVersion)
        .version("1.0")
        .arg(
            Arg::new("config")
                .short('c')
                .long("config")
                .value_name("FILE")
                .help("Sets a custom config file")
                .takes_value(true),
        )
        .subcommand(
            App::new("test")
                .about("print debug information verbosely")
                .subcommand(App::new("hello").about("hello")),
        )
        .get_matches();
}
```
`PropagateVersion` ensures you get
```console
$ ./target/debug/clap-test --version
claptest 1.0
$ ./target/debug/clap-test test --version
claptest-test 1.0
$ ./target/debug/clap-test test hello --version
claptest-test-test 1.0
```
instead of
```console
$ ./target/debug/clap-test --version
claptest 1.0
$ ./target/debug/clap-test test --version
clap-test-test 
$ ./target/debug/clap-test test hello --version
clap-test-test-hello
```

---

_Comment by @epage on 2022-02-03 14:52_

I feel like what we show could be derived from the work in #1334

---

_Referenced in [clap-rs/clap#1334](../../clap-rs/clap/issues/1334.md) on 2022-02-03 14:53_

---

_Label `S-waiting-on-design` removed by @epage on 2022-02-03 14:55_

---

_Label `S-blocked` added by @epage on 2022-02-03 14:55_

---

_Referenced in [clap-rs/clap#1474](../../clap-rs/clap/issues/1474.md) on 2022-05-04 21:40_

---

_Referenced in [dtolnay/cargo-unlock#8](../../dtolnay/cargo-unlock/pulls/8.md) on 2023-08-05 18:04_

---

_Referenced in [dtolnay/cargo-expand#189](../../dtolnay/cargo-expand/pulls/189.md) on 2023-08-05 18:16_

---

_Referenced in [astral-sh/uv#13109](../../astral-sh/uv/pulls/13109.md) on 2025-04-25 16:36_

---
