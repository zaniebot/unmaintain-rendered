---
number: 4853
title: "clap suggests hidden subcommands in \"similar subcommand\" error hints"
type: issue
state: open
author: yshui
labels:
  - C-bug
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2023-04-21T13:48:38Z
updated_at: 2024-04-23T20:34:21Z
url: https://github.com/clap-rs/clap/issues/4853
synced_at: 2026-01-10T01:28:02Z
---

# clap suggests hidden subcommands in "similar subcommand" error hints

---

_Issue opened by @yshui on 2023-04-21 13:48_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

irrelevant

### Clap Version

master

### Minimal reproducible code

```rust
fn main() {
    clap::Command::new("app")
        .subcommand(clap::Command::new("subcommand").hide(true))
        .get_matches();
}
```

### Steps to reproduce the bug with the above code

cargo run -- su

### Actual Behaviour

```
error: unrecognized subcommand 'su'

  tip: a similar subcommand exists: 'subcommand'
```

### Expected Behaviour

nothing is suggested.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @yshui on 2023-04-21 13:48_

---

_Comment by @yshui on 2023-04-21 13:48_

Fix in #4852 

---

_Comment by @epage on 2023-04-21 14:09_

Can you give your use case as to why you expect hidden subcommands to be not suggested?

The exact semantics of `hide` come up from time to time.  For example, we suggest hidden subcommands and arguments in completions (#1335).  In the new completion system, the proposal is to not suggest completing hidden items by default but to instead complete them if it looks like the user is intending to use the hidden item (#3951).

This latter case sounds similar to suggestions in errors which runs counter to this issue / #4852 which is why this merits further discussion.

---

_Comment by @yshui on 2023-04-21 14:21_

so the tool I am working on has commands that is not intended to be run manually, at all. they are only intended to be invoked internally by the tool itself. 

i am ok with only suggesting them when the user _do_ intend to use them, but i don't see how you can reliably detect user intentions.

i found this to be a problem because clap suggested things like `__subcommand` when i typed `sub`.

---

_Label `S-waiting-on-decision` added by @epage on 2023-04-21 14:24_

---

_Label `A-help` added by @epage on 2023-04-21 14:24_

---

_Comment by @yshui on 2023-04-21 14:26_

maybe there can be two levels of hidden-ness? like, hidden and _hidden_ hidden

---

_Comment by @weihanglo on 2023-08-17 22:21_

With <https://github.com/clap-rs/clap/pull/5075>, we have [`UnknownArgumentValueParser`](https://docs.rs/clap/latest/clap/builder/struct.UnknownArgumentValueParser.html) to guide users to use the correct flag. If clap provides a way to stop typo-suggesting a flag, it would be perfect!

---

_Referenced in [rust-lang/cargo#11702](../../rust-lang/cargo/issues/11702.md) on 2023-08-22 20:33_

---

_Referenced in [clap-rs/clap#5097](../../clap-rs/clap/pulls/5097.md) on 2023-08-28 17:27_

---

_Comment by @Carreau on 2023-08-28 17:52_

I was curious and tried to find a way to do so in #5097.

It adds `.didyoumean(false)` that filter out arguments from the suggestion logic. In combination with `.hide(...)` I think that would create what @weihanglo wants and we can extends for subcommands for @yshui. 

Anyway just a suggestion instead of fighting for wether or not `hide(...)` should suggest or not.

---

_Comment by @epage on 2023-08-28 18:53_

> I think that would create what @weihanglo wants

There was more discussion in rust-lang/cargo#11702 and it is unclear what we want the behavior to be in that case.

> Anyway just a suggestion instead of fighting for wether or not hide(...) should suggest or not.

Except this shifts the discussion to whether this part of the API pulls enough weight to justify itself.  We've been focusing recently on shrinking clap, in binary size, in compile time, and in API size.  The bigger the API is, the harder it is to find anything, making it less likely people will use any of the features in the API, lowering the value gained by having each piece of the API.

---

_Renamed from "clap suggests hidden subcommands" to "clap suggests hidden subcommands in errors" by @epage on 2024-01-15 22:07_

---

_Renamed from "clap suggests hidden subcommands in errors" to "clap suggests hidden subcommands in "similar subcommand" error hints" by @epage on 2024-04-23 20:34_

---

_Referenced in [clap-rs/clap#5471](../../clap-rs/clap/issues/5471.md) on 2024-04-23 20:34_

---

_Referenced in [jj-vcs/jj#5335](../../jj-vcs/jj/issues/5335.md) on 2025-01-10 20:43_

---
