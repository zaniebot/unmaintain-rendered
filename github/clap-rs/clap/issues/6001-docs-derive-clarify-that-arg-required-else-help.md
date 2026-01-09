---
number: 6001
title: "docs(derive): Clarify that `arg_required_else_help(true)` is implicitly set when they define the required subcommand"
type: issue
state: closed
author: sorairolake
labels: []
assignees: []
created_at: 2025-05-12T09:53:05Z
updated_at: 2025-05-23T18:07:54Z
url: https://github.com/clap-rs/clap/issues/6001
synced_at: 2026-01-07T13:12:20-06:00
---

# docs(derive): Clarify that `arg_required_else_help(true)` is implicitly set when they define the required subcommand

---

_Issue opened by @sorairolake on 2025-05-12 09:53_

If they define the required subcommand (`command: Commands`), `arg_required_else_help(true)` and `subcommand_required(true)` are implicitly set.

It has already been described in the documentation that `command: Commands` defines the required subcommand and `command: Option<Commands>` defines the optional subcommand. This is also evident from these types. So I don't think there is any need to describe `subcommand_required`.

I've searched the documentation, but at least in <https://docs.rs/clap/4.5.38/clap/_derive/index.html#command-attributes> and <https://docs.rs/clap/4.5.38/clap/_derive/_tutorial/index.html#subcommands>, I couldn't find any description about `arg_required_else_help(true)` being implicitly set. I didn't know that `arg_required_else_help(true)` is implicitly set until I read #3280 and #3721. I initially thought that if the required subcommand didn't exist, an error message would be displayed instead of a help message.

So I think it would be better to clarify in the documentation that `arg_required_else_help(true)` is implicitly set when they define the required subcommand (`command: Commands`).

---

_Referenced in [clap-rs/clap#6012](../../clap-rs/clap/pulls/6012.md) on 2025-05-23 18:02_

---

_Closed by @epage on 2025-05-23 18:07_

---
