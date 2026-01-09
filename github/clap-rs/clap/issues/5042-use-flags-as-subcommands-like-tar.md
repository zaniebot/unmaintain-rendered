---
number: 5042
title: "Use flags as subcommands (like `tar`)"
type: issue
state: closed
author: nickeb96
labels:
  - C-enhancement
assignees: []
created_at: 2023-07-23T15:47:27Z
updated_at: 2023-07-30T01:48:03Z
url: https://github.com/clap-rs/clap/issues/5042
synced_at: 2026-01-07T13:12:20-06:00
---

# Use flags as subcommands (like `tar`)

---

_Issue opened by @nickeb96 on 2023-07-23 15:47_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.3.19

### Describe your use case

I'd like to be able to build commands where there are multiple, mutually exclusive usages.  A good example of this is the `tar` command.

From the man page:

```
tar {-c} [options] [files | directories]
tar {-r} -f archive-file [options] [files | directories]
tar {-t | -x} [options] [patterns]
```

The flags `-c`, `-r`, `-t`, `-x` change how the rest of the arguments are parsed.  This would be easy to accomplish with clap if those flags were instead subcommands, but I haven't found a good way of doing it with flags.



### Describe the solution you'd like

One potential solution would be to allow the presence of a flag to act as a subcommand.

Another solution would be to have a usage enum that could look something like:

```rust
#[derive(Debug, clap::Usage)]
pub enum TarUsage {
    #[usage(short_present = 'c')]
    Create(CreateArgs),
    #[usage(short_present = 'x')]
    Extract(ExtractArgs),
    #[usage(short_present = 't')]
    List(ListArgs),
}
```

It would dispatch parsing of the remaining arguments to a different parser based on the presence of a flag.  The usage annotation could be expanded to set a default usage, or handle more complex conditions.

### Alternatives, if applicable

The solution I'm using now is to just have every argument for all variants in one struct and put them all in `Option<>`s.

A downside of this approach is that any code using the that struct has `.unwrap()`s everywhere.  It also requires writing a bunch of annotation rules describing when an argument is required or forbidden based on the presence of a flag.

I also need to use `#[command(override_usage = ...)` because the default usage string will be wrong.

I feel like this is a common type of command and the only approach I've found is not very good.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @nickeb96 on 2023-07-23 15:47_

---

_Comment by @epage on 2023-07-24 15:06_

We have [subcommand long flags](https://docs.rs/clap/latest/clap/_derive/_cookbook/pacman/index.html).  Though that example hasn't been translated into the derive API, builder methods can be used as derive attributes ([reference](https://docs.rs/clap/latest/clap/_derive/index.html#terminology)).

The one potential downside is that the subcommand names will remain visible (thought we had an issue for this but can't find it).  A workaround is to use a `help_template` and not show the subcommands.  #1334 would be useful though.

---

_Comment by @epage on 2023-07-24 15:07_

Alternatively, if you are willing to dispatch the positional arguments yourself, you could represent `{ -c | -r | { -t | -x } }` as an `ArgGroup` and then have their respective flags `requires` the one from the arg group.  This will fall flat if there is a flag that needs to be used differently depending on the "mode".

---

_Comment by @nickeb96 on 2023-07-30 01:34_

Thanks, using `#[command(short_flag = '...', long_flag = "...")]` on subcommand enum variants solved my initial problem.

I was also wondering if it's possible to have a default subcommand that will be chosen when a subcommand flag isn't present.

I'd also like be able to disable the non-flag version of a subcommand so they don't conflict with positional arguments.


---

_Comment by @epage on 2023-07-30 01:48_

> I was also wondering if it's possible to have a default subcommand that will be chosen when a subcommand flag isn't present.

Check out our [`git` example](https://docs.rs/clap/latest/clap/_derive/_cookbook/git_derive/index.html) as `git stash` defaults to `git stash push`.

> I'd also like be able to disable the non-flag version of a subcommand so they don't conflict with positional arguments.

I know this has come up before but not finding a relevant issue.  Feel free to create one.

Since the original answer is handled, I'm going to go ahead and close this.

---

_Closed by @epage on 2023-07-30 01:48_

---
