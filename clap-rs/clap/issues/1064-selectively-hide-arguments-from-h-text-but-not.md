---
number: 1064
title: "Selectively hide arguments from '-h' text (but not '--help')"
type: issue
state: closed
author: sharkdp
labels:
  - A-help
  - E-medium
assignees: []
created_at: 2017-10-11T21:50:45Z
updated_at: 2018-08-02T03:30:12Z
url: https://github.com/clap-rs/clap/issues/1064
synced_at: 2026-01-10T01:26:42Z
---

# Selectively hide arguments from '-h' text (but not '--help')

---

_Issue opened by @sharkdp on 2017-10-11 21:50_

I really like clap's feature to distinguish between a short and concise help text via `-h` and a longer and detailed help text via `--help` (see [`Arg::long_help`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.long_help)).

It would be great if whole arguments (e.g. options that are not commonly used) could be hidden from the `-h` text but still show up on the detailed `--help` text.

I propose to add a method like `Arg::short_hidden(self, h: bool)` or `Arg::long_visible(self, v: bool)` that could be used to configure this behavior.

---

_Label `C: help message` added by @kbknapp on 2017-10-12 18:38_

---

_Label `C: settings` added by @kbknapp on 2017-10-12 18:38_

---

_Label `D: easy` added by @kbknapp on 2017-10-12 18:38_

---

_Label `M: mentored` added by @kbknapp on 2017-10-12 18:38_

---

_Label `P4: nice to have` added by @kbknapp on 2017-10-12 18:38_

---

_Label `T: new setting` added by @kbknapp on 2017-10-12 18:38_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-12 18:38_

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:35_

---

_Comment by @stevepentland on 2018-03-04 22:28_

@kbknapp I'd like to add this, I'm just having trouble spotting where the help and long_help from Base are used when showing the help output.

---

_Comment by @kbknapp on 2018-03-04 22:38_

@stevepentland awesome! I'm on mobile so I can't give a super comprehensive answer at the moment, but look in the implementation of AnyArg for the respective Arg types (flag, positional, and opt), and they're in used help.rs which does all the help message printing.

Once I get to a computer I can give a better answer 

---

_Comment by @kbknapp on 2018-03-05 00:12_

In [`src/app/help.rs:445`](https://github.com/kbknapp/clap-rs/blob/7b1b6814f8ccd73b59d2b9a66342a45784bc7aa9/src/app/help.rs#L445-L449) is where decides which version of the help to display, notice the `use_long` in the branch. 

In [`src/app/help.rs:205`](https://github.com/kbknapp/clap-rs/blob/7b1b6814f8ccd73b59d2b9a66342a45784bc7aa9/src/app/help.rs#L205-L207) Is where it decides which args to display/hide.

I would recommend adding two methods to the `Arg` struct such as `Arg::hidden_long_help` and `Arg::hidden_short_help` using `Arg::hidden` as a guide.

This will require *also* creating corresponding `ArgSettings` variants in `src/args/settings.rs`, again using `ArgSettings::Hidden` as a guide.

The way I envision this working is setting `Arg::hidden(true)` is effectively the same as setting *both* `Arg::hidden_long_help` and `Arg::hidden_short_help`, so the docs should reflect that.


---

_Removed from milestone `2.27.0` by @kbknapp on 2018-03-05 00:14_

---

_Comment by @stevepentland on 2018-03-05 00:27_

Awesome, thanks for the guidance! I’ll start digging and hopefully have something soon.

---

_Referenced in [clap-rs/clap#1235](../../clap-rs/clap/pulls/1235.md) on 2018-03-28 19:59_

---

_Comment by @kbknapp on 2018-04-02 16:52_

Closed with #1235 thanks to @stevepentland 

---

_Closed by @kbknapp on 2018-04-02 16:52_

---
