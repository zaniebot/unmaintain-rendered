---
number: 3403
title: "short `-h` conflicts with help rather than overwriting it"
type: issue
state: closed
author: steveklabnik
labels:
  - C-bug
  - A-builder
  - E-medium
assignees: []
created_at: 2022-02-04T20:36:08Z
updated_at: 2022-04-13T08:55:48Z
url: https://github.com/clap-rs/clap/issues/3403
synced_at: 2026-01-07T13:12:19-06:00
---

# short `-h` conflicts with help rather than overwriting it

---

_Issue opened by @steveklabnik on 2022-02-04 20:36_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.57.0

### Clap Version

3.0.14

### Minimal reproducible code

```rust
use clap::IntoApp;
use clap::Parser;

#[derive(Parser)]
#[clap(name = "command")]
pub struct Args {
    #[clap(subcommand)]
    pub cmd: Subcommand,
}

#[derive(Parser)]
pub enum Subcommand {
    #[clap(external_subcommand)]
    Other(Vec<String>),
}

#[derive(Parser)]
#[clap(name = "subcommand")]
struct SubArgs {
    #[clap(short)]
    hold: bool,
}


fn main() {
    let mut clap = Args::into_app();

    let subcmd = SubArgs::into_app();

    clap = clap.subcommand(subcmd);

    let _m = clap.get_matches();
}
```

### Steps to reproduce the bug with the above code

` cargo run -- subcommand -h`

### Actual Behaviour

```
C:\Users\steve\tmp\claptest> cargo run -- subcommand -h
   Compiling claptest v0.1.0 (C:\Users\steve\tmp\claptest)
    Finished dev [unoptimized + debuginfo] target(s) in 0.55s
     Running `target\debug\claptest.exe subcommand -h`
thread 'main' panicked at 'App subcommand: Short option names must be unique for each argument, but '-h' is in use by both 'hold' and 'help'', C:\Users\steve\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-3.0.14\src\build\app\debug_asserts.rs:100:17
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: process didn't exit successfully: `target\debug\claptest.exe subcommand -h` (exit code: 101)
```

### Expected Behaviour

```
C:\Users\steve\tmp\claptest> cargo run -- subcommand -h
    Finished release [optimized] target(s) in 0.03s
     Running `target\release\claptest.exe subcommand -h`
claptest.exe-subcommand

USAGE:
    claptest.exe subcommand [OPTIONS]

OPTIONS:
    -h
    -h, --help    Print help information
```

### Additional Context

```toml
[dependencies.clap]
version = "3.0.14"
features = ["derive"]
```

https://github.com/clap-rs/clap/discussions/3400#discussioncomment-2107536

Because this is a debug assertion that fails, running with `--release` gives the expected behavior, which is what initially caused this bug to go unnoticed for a while.

I know that this setup might be *slightly* weird, this is an app I didn't originally write and there's a comment block:

```rust
    /*
     * This isn't hugely efficient, but we actually parse our arguments
     * twice: the first is with our subcommands grafted into our
     * arguments to get us a unified help and error message in the event
     * of any parsing value or request for a help message; if that works,
     * we parse our arguments again but relying on the
     * external_subcommand to directive to allow our subcommand to do any
     * parsing on its own.
     */
```

I recently ported this app from structopt/clapv2 to clap v3; it's possible that this comment is now irrelevant but I haven't investigated that yet.

### Debug Output

```text
     Running `target\debug\claptest.exe subcommand -h`
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:command
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_check_help_and_version: Building help subcommand
[            clap::build::app]  App::_propagate_global_args:command
[            clap::build::app]  App::_derive_display_order:command
[            clap::build::app]  App::_derive_display_order:subcommand
[            clap::build::app]  App::_derive_display_order:help
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:help
[clap::build::app::debug_asserts]       App::_verify_positionals
[         clap::parse::parser]  Parser::get_matches_with
[         clap::parse::parser]  Parser::get_matches_with: Begin parsing 'RawOsString("subcommand")' ([115, 117, 98, 99, 111, 109, 109, 97, 110, 100])
[         clap::parse::parser]  Parser::get_matches_with: Positional counter...1
[         clap::parse::parser]  Parser::get_matches_with: Low index multiples...false
[         clap::parse::parser]  Parser::possible_subcommand: arg=RawOsStr("subcommand")
[         clap::parse::parser]  Parser::get_matches_with: sc=Some("subcommand")
[         clap::parse::parser]  Parser::parse_subcommand
[         clap::output::usage]  Usage::get_required_usage_from: incls=[], matcher=false, incl_last=true
[         clap::output::usage]  Usage::get_required_usage_from: unrolled_reqs={}
[         clap::output::usage]  Usage::get_required_usage_from: ret_val=[]
[            clap::build::app]  App::_build
[            clap::build::app]  App::_propagate:subcommand
[            clap::build::app]  App::_check_help_and_version
[            clap::build::app]  App::_check_help_and_version: Removing generated version
[            clap::build::app]  App::_propagate_global_args:subcommand
[            clap::build::app]  App::_derive_display_order:subcommand
[clap::build::app::debug_asserts]       App::_debug_asserts
[clap::build::arg::debug_asserts]       Arg::_debug_asserts:hold
thread 'main' panicked at 'App subcommand: Short option names must be unique for each argument, but '-h' is in use by both 'hold' and 'help'', C:\Users\steve\.cargo\registry\src\github.com-1ecc6299db9ec823\clap-3.0.14\src\build\app\debug_asserts.rs:100:17
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: process didn't exit successfully: `target\debug\claptest.exe subcommand -h` (exit code: 101)
```

---

_Label `C-bug` added by @steveklabnik on 2022-02-04 20:36_

---

_Comment by @steveklabnik on 2022-02-04 20:45_

<details>
<summary>I was wrong with this comment, click to expand if you want to see how I was wrong.</summary>
<br>
My best guess:

```
[            clap::build::app]  App::_check_help_and_version: Removing generated version
```

(side note: wow that debug flag is cool!)

It appears to remove the generated version, but not the generated help. 

https://github.com/clap-rs/clap/blob/7ca872e7bbe6433c7bcb9d0d6fca955a94f46a84/src/build/app/mod.rs#L2915-L2924

vs

https://github.com/clap-rs/clap/blob/7ca872e7bbe6433c7bcb9d0d6fca955a94f46a84/src/build/app/mod.rs#L2873-L2881

by diffing the two, it seems that the difference is version has

```
            || (self.version.is_none() && self.long_version.is_none())

```

but help does not!

(also: one has `self.settings.is_set` and the other has `self.is_set`, though I don't know if that is material.)
</details>


---

_Comment by @epage on 2022-02-04 20:48_

btw a slightly simpler reproduction case
```rust
use clap::IntoApp;
use clap::Parser;

#[derive(Parser)]
#[clap(name = "command")]
pub struct Args {
    #[clap(subcommand)]
    pub cmd: Subcommand,
}

#[derive(Parser)]
pub enum Subcommand {
    #[clap(external_subcommand)]
    Other(Vec<String>),
    Subcommand {
        #[clap(short)]
        hold: bool,
    },
}

fn main() {
    let _m = Args::parse();
}
```

---

_Comment by @epage on 2022-02-04 20:50_

The goal isn't to remove help but to not set the short.  This is the relevant section of code
```rust
            let other_arg_has_short = self.args.args().any(|x| x.short == Some('h'));
            let help = self
                .args
                .args_mut()
                .find(|x| x.id == Id::help_hash())
                .expect(INTERNAL_ERROR_MSG);

            if !(help.short.is_some()
                || other_arg_has_short
                || self.subcommands.iter().any(|sc| sc.short_flag == Some('h')))
            {
                help.short = Some('h');
            }
```

---

_Label `A-builder` added by @epage on 2022-02-04 20:52_

---

_Label `E-medium` added by @epage on 2022-02-04 20:52_

---

_Comment by @epage on 2022-02-04 20:58_

We aren't setting `help.short` here because it was already set when the parent command propagated global arguments to child commands.  This is what let's users change the help text in the parent but not in child commands.

---

_Comment by @epage on 2022-02-04 21:10_

Ok, so I can detect this case and remove `help.short`.  We have two options
- Remove the built-in short when propagating so the subcommand can add it back in if needed
- When adding the short flag in, change the "is short flag already present" to "is short flag already present by user".  We don't have perfect information about this but we can get close

My question is "should we change this?".  Not from an implementation perspective but a user perspective.  This will only be hit when a child subcommand has a `-h` flag but nothing else in the command hierarchy does.  So this one subcommand will violate the user's expectations for how the app works.  I expect this would lead to user confusion and would be considered a bug by a developer.  Instead we can assert specifically on this case and tell users to either rename their argument or to override the parent command's help argument so they provide a consistent help experience across the entire app.

Granted, we don't do anything else in clap to enforce consistency but I think "help" and "version' are a little different in that regard.

Thoughts?

---

_Comment by @steveklabnik on 2022-02-04 21:28_

There's also the third option, which is "do nothing but document this in the documentation for short" :)

To be honest, I have no strong opinions about how clap handles this case; personally, I would never add in a `-h` because, to me, that means help, which is why:

> I expect this would lead to user confusion and would be considered a bug by a developer.

I agree! But also talking to some folks, I think it might be more subtle than that. When discussing this behavior with the rest of the folks involved some people apparently only think of `--help`, and not `-h`. So they were surprised that a `-h` would introduce a conflict in the first place. I am not sure how this plays out in the overall population of developers and users, though. To them, I think the "remove the built-in short when propagating so the subcommand can add it back in if needed" is probably inline with their expectations, given that they wouldn't even realize the short existed.

A tough choice! I also agree that `help` and `version` feel a bit different than other options with regards to consistency.

---

_Referenced in [clap-rs/clap#3405](../../clap-rs/clap/issues/3405.md) on 2022-02-05 04:39_

---

_Referenced in [clap-rs/clap#3425](../../clap-rs/clap/pulls/3425.md) on 2022-02-09 00:57_

---

_Closed by @epage on 2022-02-09 01:16_

---

_Comment by @johntconklin on 2022-03-29 00:50_

Sorry, I'm a bit late to this conversation.  I just discovered this issue when updating an CLI at $DAYJOB from clap 2.34.0 to 3.1.6.

> So this one subcommand will violate the user's expectations for how the app works. I expect this would lead to user confusion and would be considered a bug by a developer. Instead we can assert specifically on this case and tell users to either rename their argument

This seems a little over-opinionated.  Especially considering existing clap v2 apps where "-h" or "-h foo" mean something else for some of their subcommands; expecting/requiring devs change their app's subcommand's arguments is even more likely to violate their user's expectations.

> When discussing this behavior with the rest of the folks involved some people apparently only think of --help, and not -h

This is true in my case.  To address the consistency / confusion issue, I'd be happy with something like a Command::disable_short_help_flag() to disable just '-h'.

---

_Comment by @awused on 2022-04-13 08:55_

This change surprised me and broke my code that previously worked with structopt, and which worked for every other subcommand except one which panics at run time. Not a good experience at all, for the developer or user. Many applications, including git, only use --help and not -h, so this does seem like an unnecessary restriction.

---
