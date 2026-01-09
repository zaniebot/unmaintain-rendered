---
number: 5577
title: "the default derive for booleans should also handle explicit `--flag=true`, `--flag=false`"
type: issue
state: open
author: AaronKutch
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2024-07-09T18:37:12Z
updated_at: 2024-07-09T20:36:03Z
url: https://github.com/clap-rs/clap/issues/5577
synced_at: 2026-01-07T13:12:20-06:00
---

# the default derive for booleans should also handle explicit `--flag=true`, `--flag=false`

---

_Issue opened by @AaronKutch on 2024-07-09 18:37_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4

### Describe your use case

I searched around for related issues but only saw stuff like --flag/--no-flag which is not what this is about, or posts that were actually using this pattern in examples (I'm not the first one to need this, and have seen it elsewhere). In things such as automated scripts, sometimes I have a pattern of usage like passing `format!("... --flag={val}")`. However, if the value is a boolean, it does not allow things like `--flag=true` or `--flag=false`, only passing `--flag` by itself which requires annoying conditionals in the scripts. What I have to do is apply
```
#[clap(
      long,
      default_missing_value("true"),
      default_value("false"),
      num_args(0..=1),
      action = clap::ArgAction::Set,
)]
```
to every single boolean (note that it still allows `--flag` by itself and the absence of the flag to work as before). This is obviously quite annoying to use if there are a lot of booleans across many structs.

### Describe the solution you'd like

Simply make this the default for booleans. I feel like this is a property of some other command argument systems I've seen before, but can't remember.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @AaronKutch on 2024-07-09 18:37_

---

_Label `M-breaking-change` added by @epage on 2024-07-09 19:30_

---

_Label `A-derive` added by @epage on 2024-07-09 19:30_

---

_Comment by @epage on 2024-07-09 19:33_

As proposed, this would be inappropriate due to the [WARNING on `num_args`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.num_args).

We could add [`require_equals(true)`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.require_equals) but I'm at least not a fan of the inconsistency of sometimes `=` is needed and sometimes it isn't.

This would also be a dramatic shift away from the builder API.  This would also be too much policy to put in the builder API.

At this time, I am against this change but I'm ok with leaving this open to see what other feedback comes in, either way.  We should note that an issue like this is more likely to collect feedback in favor of it than against because only people wanting this will be actively looking for it.

---

_Label `S-waiting-on-decision` added by @epage on 2024-07-09 19:33_

---

_Comment by @AaronKutch on 2024-07-09 20:36_

The way I think about it, is that the normal form would always be to give values to flags, except that since booleans have only two states, they can be used without arguments. I see however how it would cause problems with positional arguments. Maybe some kind of shorthand that works in place of `#[derive(long)]` but adds the functionality for booleans could be added?

---

_Referenced in [kokic/kodama#14](../../kokic/kodama/issues/14.md) on 2025-03-15 18:37_

---
