---
number: 4669
title: Allow unsetting global args in some subcommands
type: issue
state: closed
author: jeertmans
labels:
  - C-enhancement
assignees: []
created_at: 2023-01-23T22:17:46Z
updated_at: 2023-01-25T15:35:12Z
url: https://github.com/clap-rs/clap/issues/4669
synced_at: 2026-01-10T01:27:59Z
---

# Allow unsetting global args in some subcommands

---

_Issue opened by @jeertmans on 2023-01-23 22:17_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.1

### Describe your use case

Hello, no sure if this is currently possible, but I'd like to be able to "unset" glob args for specific subcommand.

Let us assume the following code:

```rust
use clap::{Parser, Subcommand, Args}

#[derive(Parser, Debug)]
struct Args {
    #[args(long, global = true)]
    global_args: bool,
    #[command(subcommand)]
    command: Command,
}

#[derive(Subcommand, Debug)]
enum Command {
    A(A),
    B(B),
    Other(Other),
}

#[derive(Args, Debug)]
struct A {
    /* ... */
}

#[derive(Args, Debug)]
struct B {
    /* ... */
}

#[derive(Args, Debug)]
struct Other {
    /* ... */
}
```

where commands `A` and `B` make good use of `global_args`, but `Other` command does need any global argument. I would like to suppress them from the help output, as well as from parsing at all.

### Describe the solution you'd like

Ideally, I'd like to be able to disable some or all global args for a given subcommand, via a macro attribute or something similar:

```rust
#[derive(Args, Debug)]
#[command(unset_globals)]
struct Other {
    /* ... */
}
```

### Alternatives, if applicable

I think a way to "trick" the user would be to manually rewrite the help template, but I don't like this solution since it misses the advantage of "automated" generation.

If a solution is already possible, please help me because I could not find one :D !

### Additional Context

_No response_

---

_Label `C-enhancement` added by @jeertmans on 2023-01-23 22:17_

---

_Comment by @epage on 2023-01-23 22:30_

Generally, when I see global args that are not applicable to all subcommands, I view it as that arg shouldn't have been global.  A common reason for this happening is developers trying to "DRY" their CLI by using global when the sharing is incidental and not part of the requirements and `flatten` should be used instead.

What I would need to see from this proposal, before we weigh out if the benefits outweigh the complexity, is a solid use case for why an argument should be global with exceptions.

---

_Comment by @jeertmans on 2023-01-23 22:42_

Well maybe you flattening is indeed the solution.

I think it would be worth documenting the trade offs of global arts, and mention using flatten instead in some cases.

I’ll work on that solution tomorrow, and tell you if that can solve my problem.

In any cases, I would be glad helping to documenting a bit more this.

Thanks for the quick reply!

---

_Comment by @jeertmans on 2023-01-25 15:35_

After a few thoughts, reusing a structure for common args is not so much of a problem.

Closing this as it kind of solve my problem :)

Thank you for your time!

---

_Closed by @jeertmans on 2023-01-25 15:35_

---
