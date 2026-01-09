---
number: 5424
title: Delegate completions for a specific argument to the completions of an external program
type: issue
state: open
author: alerque
labels:
  - C-enhancement
  - A-completion
  - S-blocked
  - S-waiting-on-design
assignees: []
created_at: 2024-03-25T14:07:39Z
updated_at: 2024-03-25T18:20:30Z
url: https://github.com/clap-rs/clap/issues/5424
synced_at: 2026-01-07T13:12:20-06:00
---

# Delegate completions for a specific argument to the completions of an external program

---

_Issue opened by @alerque on 2024-03-25 14:07_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5

### Describe your use case

I have an app that has several subcommands, some of which are wrappers around other CLI tools. For example `casile` has several subcommands, but one of them is `casile make` that sets up a special environment and this passes all the remaining args to `make`. I have it setup something like this:

```rust
#[derive(Subcommand)]
pub enum Commands {
    Make {
        target: Vec<String>,
    },
}
```

The `Vec<String>` seems to be the best way to collect further arguments and pass them through to the subprocess. Since passing other flags to `make` is valid here the arguments beyond this point may or may not be filenames, and even if they are they may or may not exist yet which makes filename completion (as I would get if I made this a `Vec<PathBuf>` or similar not very useful.

### Describe the solution you'd like

I'd like to be able to specify something in the CLI that specifically called out the command for which to use completions for:

```rust
#[derive(Subcommand)]
pub enum Commands {
    Make {
        #[clap_complete(delegate="make")]
        target: Vec<String>,
    },
}
```

I don't know what the exact ergonomics should be, but somehow I'd like the final generated completion scripts to delegate completions for arguments in this position to the completion script of a different external command.

### Alternatives, if applicable

_No response_

### Additional Context

I'm not sure all supported shells have a way to handle this, but at least bash and zsh do.

---

_Label `C-enhancement` added by @alerque on 2024-03-25 14:07_

---

_Label `A-completion` added by @epage on 2024-03-25 15:09_

---

_Comment by @epage on 2024-03-25 15:12_

This is blocked on #1232

---

_Label `S-waiting-on-design` added by @epage on 2024-03-25 15:12_

---

_Label `S-blocked` added by @epage on 2024-03-25 15:12_

---

_Comment by @alerque on 2024-03-25 17:53_

I looked at that issue before opening this, but don't think it's related. Implementing this does not require dynamic completion support. Delegating to the completions of another command is not at all the same thing as using the output of a command as completions, and the plumbing needed to make shells do this is different (and simpler).

---

_Comment by @epage on 2024-03-25 18:20_

We plan to completely change our completion system, see #3166.

Doing major re-work of our existing completions to fit this is a bit of a dead end and could put us in a design bind for when we get to the end state of #3166.  

#3166 is also responsible for us tracking being able to add more dynamic completion state to arguments, like your proposed `delegate` attribute.

---

_Referenced in [clap-rs/clap#5449](../../clap-rs/clap/issues/5449.md) on 2024-04-09 03:39_

---
