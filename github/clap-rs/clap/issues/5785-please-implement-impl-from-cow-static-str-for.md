---
number: 5785
title: "Please implement impl From<Cow<'static, str>> for StyledStr"
type: issue
state: closed
author: teythoon
labels:
  - C-enhancement
  - A-builder
  - E-easy
assignees: []
created_at: 2024-10-22T10:46:36Z
updated_at: 2025-10-21T11:02:46Z
url: https://github.com/clap-rs/clap/issues/5785
synced_at: 2026-01-07T13:12:20-06:00
---

# Please implement impl From<Cow<'static, str>> for StyledStr

---

_Issue opened by @teythoon on 2024-10-22 10:46_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.20

### Describe your use case

We have a type representing a group of arguments that we can `#[command(flatten)]` into a number of subcommands.  We want to tweak the help texts for some of them, but most will use the default help text.  The default help text is a `&'static str`, but the tweaked versions would have to be heap allocated.  Now, we'd like to avoid the heap allocations for the vast majority of help texts.

### Describe the solution you'd like

We'd like to use `Arg::new(..).help(maybe_modify("the default help text"))` where `maybe_modify` returns a `Cow<'static, str>`.

### Alternatives, if applicable

The alternative is to heap allocate even the help texts we don't want to change, i.e. do `Arg::new(..).help(maybe_modify("the default help text"))` where `maybe_modify` returns a `String`.  This works today, but meh.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @teythoon on 2024-10-22 10:46_

---

_Label `A-builder` added by @epage on 2024-10-22 15:41_

---

_Label `E-easy` added by @epage on 2024-10-22 15:41_

---

_Comment by @epage on 2024-10-22 15:43_

I'm fine with someone posting a PR for this.  If we have other places where there is a `From` from either `'static` or owned, we should add it there as well.

Note: this could be worked around instead by having `maybe_modify` return a `StyledStr` and then you can call the separate `From`s for each case.

---

_Comment by @teythoon on 2024-10-23 14:19_

Returning `StyledStr` is a good idea in the mean time, thanks!

---

_Comment by @zaira-bibi on 2024-10-25 14:02_

Hey! I'd like to try my hand at this please.

---

_Referenced in [clap-rs/clap#5799](../../clap-rs/clap/pulls/5799.md) on 2024-11-01 07:28_

---

_Comment by @zaira-bibi on 2024-11-01 07:32_

Hi @epage, I've opened a PR for this issue. I'm not sure where I was supposed to write the tests for this, so guidance in that would really be appreciated. Thanks!

---

_Referenced in [clap-rs/clap#6153](../../clap-rs/clap/pulls/6153.md) on 2025-10-16 09:14_

---

_Referenced in [clap-rs/clap#6154](../../clap-rs/clap/pulls/6154.md) on 2025-10-16 09:48_

---

_Comment by @germangarces on 2025-10-21 10:03_

Hi, the request has been implemented and merged. This can be closed.

---

_Closed by @epage on 2025-10-21 11:02_

---
