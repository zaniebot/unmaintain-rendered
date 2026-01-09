---
number: 2963
title: Make colored help output more compatible with an applications overall theme
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2021-10-29T19:58:45Z
updated_at: 2025-01-13T15:12:37Z
url: https://github.com/clap-rs/clap/issues/2963
synced_at: 2026-01-07T13:12:19-06:00
---

# Make colored help output more compatible with an applications overall theme

---

_Issue opened by @epage on 2021-10-29 19:58_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.55.0 (c8dfcfe04 2021-09-06)

### Clap Version

master

### Steps to reproduce the bug with the above code

Run [enquirer](https://github.com/termapps/enquirer#multi-select-prompt) and compare the program's regular output with its `--help` (after enabling colored help)

### Actual Behaviour

The colors clash with the application's overall theme

### Expected Behaviour

The colors are tastefully align with the rest of the theme

### Additional Context

See also #2906

In discussing [the concerns raised #2845](https://github.com/clap-rs/clap/pull/2845#issuecomment-945185102) (see also https://github.com/clap-rs/clap/discussions/2906), we realized that at least one way to describe / categorize the problem with colored help is how well it meshes with the rest of an applications theme.

One of our aims is to provide a low-effort polished ("modern") experience.  A subset of our target users will polish the rest of their app over time, including color support and will run into theme compatibility issues. If we can provide both a polished low-effort experience and increase our theme compatibility, that seems like an overall win for our users.

This could be solved by manual help generation (like today or with #2913) or by providing theming support (#1790) but these increase the friction for a polished experience, making it less likely for them to do so.

If we go with changing the current coloring, it doesn't mean we can't use color it has to be a lot more selective and semantically relevant, like with errors.  Other tools at our disposal are dim, bold, underline, and italics.

---

_Label `T: bug` added by @epage on 2021-10-29 19:58_

---

_Label `C: help message` added by @epage on 2021-10-29 19:58_

---

_Comment by @ctrlcctrlv on 2021-11-16 07:53_

If you want to guarantee better default colors regardless of a user's terminal theme, you do have the TrueColor escape codes at your disposal, as supported in modern terminals like Kitty. You can also figure out if you're in a light or dark colored terminal with the XTerm escape `\e]11;?\a`, supported in even older terminals.

---

_Referenced in [epage/clapng#231](../../epage/clapng/issues/231.md) on 2021-12-06 22:23_

---

_Referenced in [clap-rs/clap#1251](../../clap-rs/clap/issues/1251.md) on 2021-12-09 18:34_

---

_Referenced in [clap-rs/clap#1456](../../clap-rs/clap/issues/1456.md) on 2021-12-09 19:56_

---

_Referenced in [clap-rs/clap#1398](../../clap-rs/clap/issues/1398.md) on 2021-12-10 16:33_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 22:07_

---

_Referenced in [clap-rs/clap#3234](../../clap-rs/clap/issues/3234.md) on 2021-12-31 12:46_

---

_Referenced in [crate-ci/cargo-release#510](../../crate-ci/cargo-release/issues/510.md) on 2022-08-08 13:59_

---

_Referenced in [clap-rs/clap#4117](../../clap-rs/clap/pulls/4117.md) on 2022-08-26 00:35_

---

_Closed by @epage on 2022-08-26 00:50_

---

_Comment by @ctrlcctrlv on 2022-08-26 11:15_

@epage Much nicer looking now. What do you think about my theming idea? No need? Or something you'd want a PR about? I have enough TTY exp to do it for sure.

---

_Comment by @epage on 2022-08-26 12:23_

Referring to using true color?

That makes it independent of the terminal's color scheme but it doesn't make it compatible with the application's theme / branding which is what spawned this.  I've also found it can cause more problems with the terminal's theme because you don't know the background color and what provides more contrast.  Light/dark detection helps but I'm hesitant for including that.

This will best be experimented with on the user's side with #3234.  If someone settles on something that is enough of an improvement and universal enough, then we can reconsider.

---

_Referenced in [clap-rs/clap#4132](../../clap-rs/clap/issues/4132.md) on 2022-08-27 02:18_

---

_Referenced in [Wilfred/difftastic#548](../../Wilfred/difftastic/issues/548.md) on 2023-08-03 18:28_

---

_Referenced in [jj-vcs/jj#5716](../../jj-vcs/jj/pulls/5716.md) on 2025-02-16 02:18_

---
