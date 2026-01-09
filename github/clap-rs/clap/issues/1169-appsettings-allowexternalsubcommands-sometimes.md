---
number: 1169
title: "AppSettings::AllowExternalSubcommands [sometimes] triggers suggestions"
type: issue
state: closed
author: yrashk
labels:
  - C-enhancement
  - A-parsing
  - E-medium
  - E-easy
assignees: []
created_at: 2018-02-08T05:05:03Z
updated_at: 2018-08-02T03:30:18Z
url: https://github.com/clap-rs/clap/issues/1169
synced_at: 2026-01-07T13:12:19-06:00
---

# AppSettings::AllowExternalSubcommands [sometimes] triggers suggestions

---

_Issue opened by @yrashk on 2018-02-08 05:05_


### Rust Version

rustc 1.23.0 (766bd11c8 2018-01-01)

### Affected Version of clap

2.29.4

### Expected Behavior Summary

The program executions to print: 

```
# 1
subcommand: z
# 2
subcommand: te
```

### Actual Behavior Summary

```
# 1
subcommand: z
# 2
error: The subcommand 'te' wasn't recognized
	Did you mean 'test'?

If you believe you received this message in error, try re-running with 'example -- te'

USAGE:
    example [SUBCOMMAND]

For more information try --help
```

### Steps to Reproduce the issue

src/main.rs:

```rust
extern crate clap;

use clap::{App, AppSettings, SubCommand};

fn main() {
    let matches = App::new("My Super Program")
        .setting(AppSettings::AllowExternalSubcommands)
        .subcommand(SubCommand::with_name("test").about("controls testing features"))
        .get_matches();

    if let Some(_) = matches.subcommand_matches("test") {
        println!("pre-defined subcommand: test");
    } else {
        let (command, _) = matches.subcommand();
        println!("subcommand: {}", command);
    }

}
```

Run with 

```
# 1
cargo run -- z
# 2
cargo run -- te
```
### Debug output

```
$ cargo run -- te
   Compiling clap v2.29.4
   Compiling example v0.1.0 (file:///Users/yrashk/tmp/example)
    Finished dev [unoptimized + debuginfo] target(s) in 8.11 secs
     Running `target/debug/example te`
DEBUG:clap:Parser::add_subcommand: term_w=None, name=test
DEBUG:clap:Parser::propagate_settings: self=My Super Program, g_settings=AppFlags(
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
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"te"' ([116, 101])
DEBUG:clap:Parser::is_new_arg:"te":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::possible_subcommand: arg="te"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:usage::create_usage_with_title;
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
DEBUG:clap:usage::create_help_usage: usage=example [SUBCOMMAND]
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::warning;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;
DEBUG:clap:Colorizer::good;
error: The subcommand 'te' wasn't recognized
	Did you mean 'test'?

If you believe you received this message in error, try re-running with 'example -- te'

USAGE:
    example [SUBCOMMAND]

For more information try --help
```

---

_Comment by @kbknapp on 2018-02-10 03:54_

Currently the fix is to not compile the `suggestions` feature.

```toml
[dependencies.clap]
version = "2"
default-features = false
features = ["color", "wrap_help"] # assuming one would still want the other defaults
```

This is a hard one to solve generally for the masses because clap has to match incoming arguments to *something* and if they happen to be close enough to trigger a "possible subcommand" there's no way for clap to know if this is what this particular use case wanted or not.

A future fix would be to add a `AppSettings::DontUseSuggestions` which would only apply to the current (sub)command. Possibly also narrowed down at a later date with the ability to only not suggest on flags/options or subcommands. I'll mark this as a todo, or good first issue for someone.

I can look at adding it in v3 if no one has knocked it out by then.

---

_Label `C: parsing` added by @kbknapp on 2018-02-10 03:55_

---

_Label `C: errors` added by @kbknapp on 2018-02-10 03:55_

---

_Label `T: enhancement` added by @kbknapp on 2018-02-10 03:55_

---

_Label `P4: nice to have` added by @kbknapp on 2018-02-10 03:55_

---

_Label `D: easy` added by @kbknapp on 2018-02-10 03:55_

---

_Label `M: mentored` added by @kbknapp on 2018-02-10 03:55_

---

_Label `T: new setting` added by @kbknapp on 2018-02-10 03:55_

---

_Label `good first issue` added by @kbknapp on 2018-02-10 03:55_

---

_Comment by @yrashk on 2018-02-10 15:58_

Kevin,

Thank you so much for the detailed explanation.

I suspect this might be a good solution for my use case where I'm trying
(in the absence of a matching subcommand) to see if I can execute
myprog-given-subcommand. What I was trying to accomplish was along the
lines of "try to match these subcommands without suggestions first, if it
fails, try finding the external binary; if there is none, enable
suggestions and run matching again". But obviously this doesn't work well
when suggestions are enabled or disabled compile time.

I'll try to see if I can prepare a patch for this.


On Feb 10, 2018 10:54 AM, "Kevin K." <notifications@github.com> wrote:

Currently the fix is to not compile the suggestions feature.

[dependencies.clap]version = "2"default-features = falsefeatures =
["color", "wrap_help"] # assuming one would still want the other
defaults

This is a hard one to solve generally for the masses because clap has to
match incoming arguments to *something* and if they happen to be close
enough to trigger a "possible subcommand" there's no way for clap to know
if this is what this particular use case wanted or not.

A future fix would be to add a AppSettings::DontUseSuggestions which would
only apply to the current (sub)command. Possibly also narrowed down at a
later date with the ability to only not suggest on flags/options or
subcommands. I'll mark this as a todo, or good first issue for someone.

I can look at adding it in v3 if no one has knocked it out by then.

—
You are receiving this because you authored the thread.
Reply to this email directly, view it on GitHub
<https://github.com/kbknapp/clap-rs/issues/1169#issuecomment-364623723>,
or mute
the thread
<https://github.com/notifications/unsubscribe-auth/AAABxMeWT1w2HZukDQ_xd1TU2mefKEl0ks5tTRLqgaJpZM4R9zl2>
.


---

_Comment by @kbknapp on 2018-02-10 19:02_

Awesome! If you'd like to try this issue, I'd suggest adding a new setting (by following the examples of all other settings) in [`src/app/settings.rs`](https://github.com/kbknapp/clap-rs/blob/4005d93df952510c2dec98e48eb8eefd2b6502e4/src/app/settings.rs#L49), then to actually *use* this setting, you'd probably check if this setting was used in [`src/app/parser.rs`](https://github.com/kbknapp/clap-rs/blob/4005d93df952510c2dec98e48eb8eefd2b6502e4/src/app/parser.rs#L657) at the start of `possible_subcommand` and just return `(false, None)` if that setting was used.

I'd also add some tests to [`tests/app_settings.rs`](https://github.com/kbknapp/clap-rs/blob/master/tests/app_settings.rs) to ensure it's all working correctly.

---

_Comment by @yrashk on 2018-04-05 14:22_

It looks like 2.31.2 solves this issue (#1215), at least to some degree. I was just able to use it the way I intended to:

---

_Closed by @yrashk on 2018-04-05 14:22_

---
