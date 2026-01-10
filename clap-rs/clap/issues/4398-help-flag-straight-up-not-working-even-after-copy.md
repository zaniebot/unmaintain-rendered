---
number: 4398
title: "`--help` flag straight up not working even after copy-pasting example"
type: issue
state: closed
author: tomaka
labels:
  - C-bug
assignees: []
created_at: 2022-10-18T06:18:44Z
updated_at: 2022-10-19T16:07:56Z
url: https://github.com/clap-rs/clap/issues/4398
synced_at: 2026-01-10T01:27:54Z
---

# `--help` flag straight up not working even after copy-pasting example

---

_Issue opened by @tomaka on 2022-10-18 06:18_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

v1.64.0

### Clap Version

v4.0.15

### Minimal reproducible code

I copy-pasted the first example of [this page](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#configuring-the-parser), however all examples from that page that I've tried exhibit the same problem. I didn't manage to write a code where the `--help` flag worked.

```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "MyApp")]
#[command(author = "Kevin K. <kbknapp@gmail.com>")]
#[command(version = "1.0")]
#[command(about = "Does awesome things", long_about = None)]
struct Cli {
    #[arg(long)]
    two: String,
    #[arg(long)]
    one: String,
}

fn main() {
    let cli = Cli::parse();

    println!("two: {:?}", cli.two);
    println!("one: {:?}", cli.one);
}
```


### Steps to reproduce the bug with the above code

`cargo run -- --help`

Or `cargo build` followed with `./target/debug/foo --help`

### Actual Behaviour

> error: Found argument '--help' which wasn't expected, or isn't valid in this context

### Expected Behaviour

It should print some help, like explained in the page:

```
Does awesome things

Usage: 02_apps_derive[EXE] --two <TWO> --one <ONE>

Options:
      --two <TWO>  
      --one <ONE>  
  -h, --help       Print help information
  -V, --version    Print version information

```

### Additional Context

I realize that this issue is similar to https://github.com/clap-rs/clap/issues/4392, but in this case, I don't know, it just never works. I also very confused, because the `--help` is the 101 of what this library does, I wouldn't expect it to straight up not work in all scenarios.
Interestingly, however, the `--version` flag works.

### Debug Output

```
[      clap::builder::command]  Command::_do_parse
[      clap::builder::command]  Command::_build: name="MyApp"
[      clap::builder::command]  Command::_propagate:MyApp
[      clap::builder::command]  Command::_check_help_and_version:MyApp expand_help_tree=false
[      clap::builder::command]  Command::long_help_exists
[      clap::builder::command]  Command::_check_help_and_version: Building default --version
[      clap::builder::command]  Command::_propagate_global_args:MyApp
[clap::builder::debug_asserts]  Command::_debug_asserts
[clap::builder::debug_asserts]  Arg::_debug_asserts:two
[clap::builder::debug_asserts]  Arg::_debug_asserts:one
[clap::builder::debug_asserts]  Arg::_debug_asserts:version
[clap::builder::debug_asserts]  Command::_verify_positionals
[        clap::parser::parser]  Parser::get_matches_with
[        clap::parser::parser]  Parser::get_matches_with: Begin parsing 'RawOsStr("--help")' ([45, 45, 104, 101, 108, 112])
[        clap::parser::parser]  Parser::possible_subcommand: arg=Ok("--help")
[        clap::parser::parser]  Parser::get_matches_with: sc=None
[        clap::parser::parser]  Parser::parse_long_arg
[        clap::parser::parser]  Parser::parse_long_arg: Does it contain '='...
[        clap::parser::parser]  Parser::possible_long_flag_subcommand: arg="help"
[        clap::parser::parser]  Parser::get_matches_with: After parse_long_arg NoMatchingArg { arg: "help" }
[        clap::parser::parser]  Parser::did_you_mean_error: arg=help
[        clap::parser::parser]  Parser::did_you_mean_error: longs=["two", "one", "version"]
[         clap::output::usage]  Usage::create_usage_with_title
[         clap::output::usage]  Usage::create_usage_no_title
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
[      clap::builder::command]  Command::color: Color setting...
[      clap::builder::command]  Auto
error: Found argument '--help' which wasn't expected, or isn't valid in this context
```

---

_Label `C-bug` added by @tomaka on 2022-10-18 06:18_

---

_Referenced in [paritytech/smoldot#2879](../../paritytech/smoldot/pulls/2879.md) on 2022-10-18 06:23_

---

_Comment by @Peternator7 on 2022-10-19 14:01_

*Came to github trying to debug the same issue.

I ran into this same problem yesterday as well, and I fixed it by enabling the `help` feature flag on the crate. I think it's surprising that this can be disabled because it is one of the main selling points to using clap.

---

_Comment by @epage on 2022-10-19 14:52_

Yes, we broke out help generation into a feature flag.  In the [migration guide](https://github.com/clap-rs/clap/blob/master/CHANGELOG.md#migrating), we have this comment about it

>  Update feature flags
>
> - If default-features = false, run cargo add clap -F help,usage,error-context
> - Run cargo add clap -F wrap_help unless you want to hard code line wraps

> I think it's surprising that this can be disabled because it is one of the main selling points to using clap.

While a main selling point, it isn't the only selling point and people have been wanting flexibility on this for a while.

For example, I am considering snapshotting my help in tests and building that in so that I have semi-automated help generation without the costs.

What I'm really looking forward to is https://github.com/rust-lang/cargo/issues/3126 which will make it easier to piece meal remove features from clap.

As this was most likely a missing `help` feature flag, I'm going to go ahead and close this.  If there is something lingering in this, let me know and I can re-open.

---

_Closed by @epage on 2022-10-19 14:52_

---

_Comment by @tomaka on 2022-10-19 15:05_

Ah, thanks for the hindsight.

I find it extremely surprising that activating/disabling a feature flag modifies the behavior of the code. For example, if you depend on `clap` with `default-features = false`, then add a dependency `foo` which itself depends on `clap` but without `default-features = false`, the behavior of your program will be modified just by adding this `foo` dependency.

If I can suggest a way better to do it, the user should explicitly attach the `#[command(help)]` attribute if they want the auto-generated help, and it would fail to compile if the feature flag is disabled.


---

_Comment by @epage on 2022-10-19 15:30_

> If I can suggest a way better to do it, the user should explicitly attach the #[command(help)] attribute if they want the auto-generated help, and it would fail to compile if the feature flag is disabled.

I think this comes back to the other comment:

> I think it's surprising that this can be disabled because it is one of the main selling points to using clap.

I'm more worried about the out-of-box experience than the `default-features = false` user, especially as I see `default-features = false` going away in the long term.

---

_Comment by @tomaka on 2022-10-19 15:41_

(sorry if I sound pushy, I'm not actually opinionated about this, happy to end the discussion at any point)

Personally I automatically put `default-features = false` on every single dependency I add. I've wasted so much time fighting with annoying feature flags interactions in the general Rust ecosystem in the past that I've decided to do this as a policy.

For this reason, I also don't provide feature flags for my (more recent) crates apart from `std`. I think that the trade-off between "improved compilation time" (having feature flags) and "increased clarity for the user" (not having feature flags) weights heavily on the side of not having feature flags at all.

I really see feature flags as one of the greatest mistakes of the Rust language in general.

> I see default-features = false going away in the long term.

I have the feeling that this is probably a decade away, if ever.

---

_Comment by @epage on 2022-10-19 16:07_

> I really see feature flags as one of the greatest mistakes of the Rust language in general.

I wonder how much of this is features themselves vs the UX not being fully developed.  For example, I've found `cargo add`, especially when crates use the `dep:` prefix to hide optional dependencies, a big help.

For a lot of feature uses, I do hope https://github.com/rust-lang/rfcs/pull/3243# goes through as an alternative.

> > I see default-features = false going away in the long term.
> I have the feeling that this is probably a decade away, if ever.

Maybe I'm being optimistic but I'm hoping for next edition.  The cargo team is interested in solving the "adding new default features without breaking" problem and several of us on the cargo team are particularly hopeful for this solution.  I don't know about others but I've just not moved down my priority list enough to get to writing an RFC for it.

---

_Referenced in [clap-rs/clap#4405](../../clap-rs/clap/issues/4405.md) on 2022-10-19 22:18_

---
