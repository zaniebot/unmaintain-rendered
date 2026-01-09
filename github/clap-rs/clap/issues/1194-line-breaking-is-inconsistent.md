---
number: 1194
title: Line breaking is inconsistent
type: issue
state: closed
author: spacekookie
labels: []
assignees: []
created_at: 2018-02-25T01:32:14Z
updated_at: 2018-08-02T03:30:19Z
url: https://github.com/clap-rs/clap/issues/1194
synced_at: 2026-01-07T13:12:19-06:00
---

# Line breaking is inconsistent

---

_Issue opened by @spacekookie on 2018-02-25 01:32_

### System info

* rustc 1.25.0-nightly (b1f8e6fb0 2018-02-22)
* clap 2.30.0

### Expected Behavior Summary

I've noticed some inconsistencies in the line breaking behaviour for long descriptions. When creating a subcommand or argument I would expect it always to wrap to the size of the terminal, then pad the text to line up with the previous line.

### Actual Behavior Summary

In actuality it sometimes feels arbitrary how lines are wrapped, almost like some parameters aren't being considered.

1. I've noticed that when my terminal is ~80 characters that long lines always overflow into the next line without being padded
2. I've also noticed that adding additional flags like `long` and `short` doesn't impact the line breaking distance.

### Steps to Reproduce the issue

Looking at some examples...

```rust
SubCommand::with_name("cmd")
  .about("A command that does things with much ferocity")
  .arg(
      Arg::with_name("type")
          .help("You can choose between different things that do almost \
          different things but not really because I'm lazy :)")
          .possible_values(&["meh", "awesome"])
          .default_value("meh"),
  ),
```
It generates the following ouput on an 80-character wide terminal

![](https://i.imgur.com/nJaVXyH.png)

When we add some more flags to the argument it get's even more funky :sweat_smile: 

```rust
SubCommand::with_name("cmd")
  .about("A command that does things with much ferocity")
  .arg(
      Arg::with_name("type")
          .short("t")
          .long("typical-argument-name")
          .help("You can choose between different things that do almost \
          different things but not really because I'm lazy :)")
          .possible_values(&["meh", "awesome"])
          .default_value("meh"),
  ),
```

![](https://i.imgur.com/MpfcAx3.png)

Granted, my use cases here *are* kinda corner-case-y. But it would be cool if clap could still handle them

### Debug output

I only ran the last example I gave with the debug output on. It can be found here: https://pastebin.com/CVt2xgG9

---

_Comment by @kbknapp on 2018-02-25 01:47_

As per the gutter discussion I'm on mobile will check this out tomorrow  :) thanks for reporting!

Also, which OS are you using, and did you compile with any non-default cargo features?

---

_Closed by @kbknapp on 2018-02-25 17:14_

---

_Comment by @kbknapp on 2018-02-25 17:14_

For those that find this issue, the fix is to compile with the `wrap_help` feature.

---
