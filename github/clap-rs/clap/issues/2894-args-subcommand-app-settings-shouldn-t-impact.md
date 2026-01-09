---
number: 2894
title: "Args/Subcommand app settings shouldn't impact what they are flattened into"
type: issue
state: closed
author: epage
labels:
  - C-bug
  - M-breaking-change
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2021-10-16T15:03:26Z
updated_at: 2022-02-08T23:58:08Z
url: https://github.com/clap-rs/clap/issues/2894
synced_at: 2026-01-07T13:12:19-06:00
---

# Args/Subcommand app settings shouldn't impact what they are flattened into

---

_Issue opened by @epage on 2021-10-16 15:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Minimal reproducible code

```rust
use clap::{Args, Parser};

#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(flatten)]
    common: Common,
}

#[derive(Args, PartialEq, Debug)]
#[clap(version = "3.0.0")]
struct Common {
    arg: i32,
}

fn main() {
    Opt::parse();
}
```


### Steps to reproduce the bug with the above code

`cargo run -- --help`

### Actual Behaviour

```
test-clap 3.0.0

USAGE:
    test-clap <ARG>

ARGS:
    <ARG>    

OPTIONS:
    -h, --help       Print help information
    -V, --version    Print version information
```

### Expected Behaviour

```
test-clap 

USAGE:
    test-clap <ARG>

ARGS:
    <ARG>    

OPTIONS:
    -h, --help    Print help information
```

### Additional Context

With the vocabulary we are providing users, we are `flatten`ing an `Args`.  This says nothing about flattening an `App` as well.

This has also led to a series of bugs like #2527.  We've worked around this by carefully setting the precedence (https://github.com/TeXitoi/structopt/pull/325, #2531 and #2819) which has lead to other bugs like #2785 which required more precedence tweaks (#2882).

By distinguishing what app settings are tightly associated with flattening (help_heading, the implicit subcommand required else help) and making everything else only apply when creating an App, we can simplify, clarify, and make less brittle these relationships.

### Debug Output

_No response_

---

_Label `T: bug` added by @epage on 2021-10-16 15:03_

---

_Label `C: derive macros` added by @epage on 2021-10-16 15:03_

---

_Comment by @epage on 2021-10-16 15:05_

Note: we can't just move the app method calls to `IntoApp` because that will break subcommands because `Args` and `Subcommand` are also implicitly sub-parsers in subcommands and we expect them to be able to initialize the subcommand's app settings.

Options
- Have `Args` and `Subcommand` also derive `IntoApp`
- Have users explicitly derive `IntoApp` when they want to be used as a subcommand
- Split the `Args` and `Subcommand` `augment_app` methods into `augment_app` and `add_args`.

---

_Referenced in [clap-rs/clap#2819](../../clap-rs/clap/pulls/2819.md) on 2021-10-16 15:36_

---

_Referenced in [clap-rs/clap#2882](../../clap-rs/clap/pulls/2882.md) on 2021-10-16 15:36_

---

_Referenced in [clap-rs/clap#2898](../../clap-rs/clap/issues/2898.md) on 2021-10-16 20:30_

---

_Label `E: breaking change` added by @epage on 2021-10-16 20:44_

---

_Comment by @epage on 2021-11-02 17:57_

In thinking about this, it might make sense for `subcommand` to pull in some appsettings.  We already implicitly add one we'd need to keep and the user might want to do other related ones like `UseLongFormatForHelpSubcommand`, `InferSubcommands`, 

---

_Referenced in [clap-rs/clap#2983](../../clap-rs/clap/issues/2983.md) on 2021-11-02 18:06_

---

_Referenced in [epage/clapng#223](../../epage/clapng/issues/223.md) on 2021-12-06 22:18_

---

_Referenced in [epage/clapng#232](../../epage/clapng/issues/232.md) on 2021-12-06 22:23_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 22:02_

---

_Referenced in [clap-rs/clap#3175](../../clap-rs/clap/pulls/3175.md) on 2021-12-14 17:04_

---

_Comment by @epage on 2021-12-14 18:05_

In #2983 I made this comment
> Earlier I had commented that #2894 is "This is bigger and riskier.". As I [mentioned](https://github.com/clap-rs/clap/issues/2894#issuecomment-957992808) in #2894, there are some settings that are coupled to a `Subcommand`s behavior, like `UseLongFormatForHelpSubcommand`, `InferSubcommands`, or help heading defaults, where it would make sense to set these on the `Subcommand` to centralize all of the subcommand policy.
> 
> Similarly, for `Args`, we have some settings on what is flattened, like `help_heading`. #1887 is one example of where we might extend that.
> 
> I'm concerned that trying to manage what can and can't be set in a `Subcommand` or an `Args` is going to take a decent amount of code, will be brittle as we forget to update this as new app settings become available, and when someone creatively comes up with a new idea, they will be blocked until a new clap release loosens the restrictions (looking at you, `clap_derive` inferred vs explicit error reporting that we've been slowly loosening)
> 
> So I wonder if we should reject #2894.


---

_Label `S-waiting-on-design` removed by @epage on 2022-02-08 23:57_

---

_Label `S-wont-fix` added by @epage on 2022-02-08 23:57_

---

_Comment by @epage on 2022-02-08 23:58_

After having had time with this in various scenarios, I feel like any solution for this will end up being worse than the problem.

---

_Closed by @epage on 2022-02-08 23:58_

---
