---
number: 3056
title: Parse arguments before or after subcommands ambivalently
type: issue
state: closed
author: 9999years
labels:
  - C-enhancement
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2021-12-02T21:14:56Z
updated_at: 2022-01-11T18:27:28Z
url: https://github.com/clap-rs/clap/issues/3056
synced_at: 2026-01-07T13:12:19-06:00
---

# Parse arguments before or after subcommands ambivalently

---

_Issue opened by @9999years on 2021-12-02 21:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

2.34, but I don't think this is a feature in 3.0 either.

### Describe your use case

I'd like to be able to pass args at any position before or after subcommands. This is a quality-of-live improvement for users of command-line tools.

Consider the following `App`:

```rust
use clap::{App, Arg, Subcommand};
App::new("prog")
    .arg(Arg::with_name("a").short("a"))
    .subcommand(SubCommand::with_name("cmd").arg(Arg::with_name("b").short("b")))
```

`prog -a cmd -b` will parse, but `prog -a -b cmd` and `prog cmd -a -b` will not. In my opinion, this makes the command-line interface more cumbersome:
- In the shell, it's harder to work on a previous command by hitting `^P` and adding options to the end -- you have to know what level the option can be set at and insert it at the correct place in the command line
- Requiring arguments at a certain position doesn't enable a compelling use-case -- I don't like the idea of a CLI where `prog -a cmd` and `prog cmd -a` mean different things

### Describe the solution you'd like

Adding a `clap::AppSettings` variant seems like the natural way to alter parsing behavior:

```rust
App::new("prog")
    .setting(AppSettings::ArgsBeforeOrAfterSubcommands)
    // ...
```

### Alternatives, if applicable

A more granular API could use an `ArgSettings` instead:

```rust
App::new("prog")
    .arg(Arg::with_name("a").short("a").set(ArgSettings::BeforeOrAfterSubcommands))
```

### Additional Context

I'm not familiar with clap's implementation, and it occurs to me that the parser might be structured such that it would be difficult or resource-intensive to alter the parser to recognize subcommand args before the subcommand token itself.

The parser would have to know about and check all the subcommand-args before parsing, or otherwise determine which subcommand is going to be used -- which I think may be tricky, because determining if `bar` is a subcommand in `prog --foo bar` would require knowing if `--foo` takes a value or not.

This change would also make certain patterns that are currently allowed ambiguous, such as having a top-level argument and a subcommand argument with the same name (and impossible to parse if, for example, one takes a value and the other doesn't).

My immediate questions about this feature request are:
- Would it be possible to add this to clap?
- Would this feature be allowed in clap, in light of the above complications?
- Should I work on a PR to implement this functionality?

---

_Label `T: new feature` added by @9999years on 2021-12-02 21:14_

---

_Comment by @epage on 2021-12-02 21:26_

While not quite what you are asking for,
- Clap does have `Arg::global` for saying a top-level argument can exist within subcommands
- You could skip clap's built-in subcommand support and define all of your arguments at the top level and have a positional argument that is your "subcommand".  You could even get some validation working with `required_if`, `conflicts_with`, etc.  In clap3 we also expose a `App::error` for custom validation to report errors like clap does.

(leaving it to others to weigh in on whether this is desirable or not)

---

_Referenced in [epage/clapng#245](../../epage/clapng/issues/245.md) on 2021-12-06 23:13_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:07_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:07_

---

_Comment by @pksunkara on 2021-12-09 08:31_

@9999years Are you migrating an existing CLI or is this a new one you are building?

---

_Comment by @9999years on 2021-12-09 19:10_

@epage Thank you, I didn't notice `Arg::global` -- that solves 90% of this!

@pksunkara I'm building a new CLI.

Setting the top-level `App`'s arguments to `global` fixes most of the problem (I think `prog cmd -a -b` is more important than `prog -a -b cmd` because command-line interfaces tend to be edited by appending to the end of a previously entered command). It would still be nice to support `prog -a -b cmd`, but as pointed out above I'm not sure how feasible that is.

---

_Comment by @epage on 2021-12-10 21:15_

In general, I would recommend following existing conventions for CLIs.  This means that flags are scoped to come directly after the command they are semantically related to.  `global` breaks from that slightly to help when you want an arg everywhere, like `--version`, `--help`, or logging flags.

Separate from that, for someone to add this to clap, we would need a special parse mode where we parse ahead to find what command we are in and then parse the flags.  This will interfere with positional argument support and can cause problems if a command and subcommand have overlapping flags (even figuring out what flags belong to which will take some work).  There is enough complexity to implementing and supporting this and enough gotchas, that I'd be concerned about moving forward with it.

---

_Comment by @9999years on 2021-12-13 17:40_

>  There is enough complexity to implementing and supporting this and enough gotchas, that I'd be concerned about moving forward with it.

@epage That makes sense, thanks for your input. Closing this as infeasible.

---

_Closed by @9999years on 2021-12-13 17:40_

---

_Label `A-parsing` added by @epage on 2022-01-11 18:27_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:27_

---

_Referenced in [rust-lang/cargo#14858](../../rust-lang/cargo/issues/14858.md) on 2024-11-26 17:24_

---
