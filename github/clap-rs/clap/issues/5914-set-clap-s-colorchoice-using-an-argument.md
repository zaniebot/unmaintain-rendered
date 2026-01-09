---
number: 5914
title: "Set clap's `ColorChoice` using an argument"
type: issue
state: open
author: acuteenvy
labels:
  - C-enhancement
assignees: []
created_at: 2025-02-18T14:45:02Z
updated_at: 2025-05-30T12:27:37Z
url: https://github.com/clap-rs/clap/issues/5914
synced_at: 2026-01-07T13:12:20-06:00
---

# Set clap's `ColorChoice` using an argument

---

_Issue opened by @acuteenvy on 2025-02-18 14:45_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.30

### Describe your use case

A lot of command-line programs support a `--color` argument to control colored output. There is no way, however, to control the color of clap-generated help and error messages using such an option.


### Describe the solution you'd like

```rust
use clap::{ColorChoice, Parser};

#[derive(Parser)]
//#[command(color = ???)]
struct Cli {
    // Somehow make clap use this value, so that if a user runs 'myprogram --color never --help'
    // for example, clap will display the help without colors.
    #[arg(long, value_name = "WHEN", default_value_t = ColorChoice::default())]
    color: ColorChoice,
}

fn main() {
    let _cli = Cli::parse();
}
```


### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @acuteenvy on 2025-02-18 14:45_

---

_Comment by @epage on 2025-02-18 16:43_

This gets into a weird chicken and egg problem

```console
$ foo --color=never --hel
```
Could parse `--color` and have it affect the error
```console
$ foo --hel --color=never
```
would hit the error before we parse `--color=never`.  Parsing of a CLI is context-sensitive.  We could have a heuristic to try to guess the intent but it would just be that.

That is leaving aside the challenges we've had with built-in vs custom `--help` and then trying to extend that to `--color`.  Probably the worst example I can think of is ripgrep (independent of whether it uses clap today) is it supports `none`, `never`, `always`, and `ansi`

I know at least Cargo is tracking this in rust-lang/cargo#9012

---

_Comment by @acuteenvy on 2025-02-18 18:10_

So, when `--color` is the first argument, it would be possible to apply it to errors and `--help`, right? That's a lot better than not having it affect clap at all.

Would it be possible for clap to support this?

---

_Comment by @epage on 2025-02-18 18:18_

I'm not sure it is better as the inconsistency in behavior may surprise people.  There is also the problem that `--help` is an error when encountered and so `--help --color=never` would be ignored.  This *could* change but that is cascading to be a lot bigger of a change and still doesn't resolve the other problems (like knowing about the flag in the first place).

---

_Comment by @acuteenvy on 2025-02-18 18:30_

Alright, thanks for the quick answers. Should I close this issue, or is it something you'd like to reconsider in a future clap release?

---

_Comment by @epage on 2025-02-18 18:37_

Unsure.  Might be good to be open for a bit to raise visibility or any other thoughts or input.

---

_Comment by @pksunkara on 2025-05-30 12:27_

FWIW, all the scenarios (writing to file, `NO_COLOR=1` etc..) where we don't want color for error messages are automatically taken care of by clap. The error and help message don't have colors then.

---
