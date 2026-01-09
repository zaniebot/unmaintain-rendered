---
number: 5055
title: "Variant of allow_hyphen_values that treats `--` like a regular value"
type: issue
state: open
author: RalfJung
labels:
  - C-enhancement
  - A-parsing
  - E-medium
assignees: []
created_at: 2023-07-30T20:30:43Z
updated_at: 2024-08-06T08:51:24Z
url: https://github.com/clap-rs/clap/issues/5055
synced_at: 2026-01-07T13:12:20-06:00
---

# Variant of allow_hyphen_values that treats `--` like a regular value

---

_Issue opened by @RalfJung on 2023-07-30 20:30_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.7

### Describe your use case

I would like to write a program with various subcommands that mostly wrap various `cargo` commands. So for instance `./miri clippy <args>` should just forward all `<args>` completely unaltered to the underlying `cargo clippy` invocations. I was hoping that enabling `trailing_var_arg` and `allow_hyphen_values` achieves this, but sadly that is not the case: `./miri clippy -- -D warnings` leads to `-D warnings` being forwarded, the leading `--` is dropped.

Interestingly, `./miri clippy --all-features -- -D warnings` behaves correctly, and `--` ends up as part of the arguments. Only a leading `--` is not handled as desired.

```rust
use clap::Parser;

#[derive(Parser)]
pub struct Args {
    #[arg(trailing_var_arg = true, allow_hyphen_values = true)]
    pub args: Vec<String>,
}

fn main() {
    let args = Args::parse_from(["program", "--all-features", "--", "-D", "warnings"]);
    println!("{:?}", args.args); // ["--all-features", "--", "-D", "warnings"]: exactly what I want
    let args = Args::parse_from(["program", "--", "-D", "warnings"]);
    println!("{:?}", args.args); // ["-D", "warnings"]: I was hoping for ["--", "-D", "warnings"]
}
```

### Describe the solution you'd like

Some way to indicate that an argument should just consume all remaining tokens into a `Vec<String>` would be great. I first thought maybe that's what `raw` does, but raw seems to actually *require* a `--` (and then strips it) so that doesn't help.

### Alternatives, if applicable

_No response_

### Additional Context

This is basically re-opening https://github.com/clap-rs/clap/issues/1236.

---

_Label `C-enhancement` added by @RalfJung on 2023-07-30 20:30_

---

_Referenced in [clap-rs/clap#1236](../../clap-rs/clap/issues/1236.md) on 2023-07-30 20:30_

---

_Comment by @epage on 2023-07-31 20:49_

Generally, a (sub)command will have some of its own arguments besides those that get forwarded.

When it doesn't, external subcommands might be more appropriate (though custom help output might be needed to get quality output).  Maybe there are things we can do for that help output...

When it does, the general approach is what you did with `trailing_var_arg(true).allow_hyphen_values(true)` as this will allows built-in arguments to be used but to start capturing as soon as the first non-built-in argument is used.  Clap handling an initial `--` is important so users can "escape" arguments that are meant to be forwarded vs the built-in.  I hope this explains why the initial `--` isn't captured but the later `--` is captured.

I'm assuming the request here is a way to completely forward a command with good help output.   `trailing_var_arg(true).allow_hyphen_values(true)` is only a solution if you also do `disable_help_flag(true)` unless you are intentionally wanting to provide your own help for the command.

At this point, it might be good to get further clarification on the use case before diving more into potential solutions.

---

_Referenced in [rust-lang/miri#3008](../../rust-lang/miri/pulls/3008.md) on 2023-08-03 12:48_

---

_Comment by @RalfJung on 2023-08-04 17:29_

> Generally, a (sub)command will have some of its own arguments besides those that get forwarded.

Yes. I would like it to do that, and then stop parsing on the first unexpected token and put all the rest into the trailing argument.

>  Clap handling an initial -- is important so users can "escape" arguments that are meant to be forwarded vs the built-in. I hope this explains why the initial -- isn't captured but the later -- is captured.

Yes I know I am losing generality here. For instance `./miri test` should take a `--bless` flag that is interpreted by the script, but everything else is forwarded. So in general I might need `./miri test -- --bless` to indicate that it should please forward `--bless` (put it in the trailing vararg) rather than interpreting it as a flag itself. I am okay with breaking that case -- the arguments after the `--` are passed to `cargo test` and `--bless` is not a valid flag there. I'd rather lose the ability to pass `--bless` into the vararg than having to write `./miri test -- -- filter1 filter2` to invoke `cargo test -- filter1 filter2`.

> At this point, it might be good to get further clarification on the use case before diving more into potential solutions.

Basically: having a script wrap another command (that already takes `--`, such as in `cargo test -- filter1 filter2` or `cargo clippy -- -D warnings`) with perfect argument forwarding, *without* requiring another nesting of `--`. I am aware that this loses some expressivity, but that still seems preferable over `-- --`.

---

_Comment by @joshtriplett on 2023-08-05 17:53_

I'm interested in this as well. I have a subcommand that takes a few options and then the name and arguments for another program. It's fine if program names starting with `-` are more awkward and require `./` or similar. I want `--` in the trailing arguments to be passed to the command verbatim.

---

_Comment by @epage on 2023-08-08 02:38_

Thanks for the explanations!

The part i find messy here is that the way the command wrapping is being done here is trying to be transparent to the user but it requires all new arguments to be placed first.

That only weighs in depending on how the options look.

In this case, I think the way this makes the most sense is the ability to turn off escaping, so `--` doesn't get treated specially by clap and it can naturally be consumed by `allow_hyphen_values`.  This can then also tie into other areas like error reporting (e.g. #2766).

---

_Label `A-parsing` added by @epage on 2023-08-08 02:39_

---

_Label `E-medium` added by @epage on 2023-08-08 02:39_

---

_Referenced in [rust-lang/miri#3592](../../rust-lang/miri/issues/3592.md) on 2024-05-09 13:28_

---

_Comment by @39555 on 2024-08-06 08:51_

As a use case: a lot of unix tools can work with `--` as an argument. For example:
```sh
# as a placeholder for the command_name
sh -c 'echo $0 $1' -- arg
#> -- arg

echo --
#> --
```
However with clap now in projects such as
- `uecho` https://github.com/uutils/coreutils
- `echo` and command line `brush -c ... --` https://github.com/reubeno/brush

the hyphen `--` is missing despite `allow_hyphen_values`  being set:
```sh
brush -c 'echo $0 $1' -- arg
#> arg

uecho --
#>

```

---

_Referenced in [reubeno/brush#147](../../reubeno/brush/pulls/147.md) on 2024-08-06 08:56_

---

_Referenced in [rust-lang/miri#4036](../../rust-lang/miri/pulls/4036.md) on 2024-11-17 02:48_

---
