---
number: 5653
title: Ability to forward completions in Rust native completions
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - E-hard
  - A-completion
assignees: []
created_at: 2024-08-10T00:16:52Z
updated_at: 2024-08-29T17:57:38Z
url: https://github.com/clap-rs/clap/issues/5653
synced_at: 2026-01-07T13:12:20-06:00
---

# Ability to forward completions in Rust native completions

---

_Issue opened by @epage on 2024-08-10 00:16_

- `cargo run`
- `cargo <plugin>`

and likely other cases require completing other commands.

See https://carapace-sh.github.io/carapace-bin/spec/embed.html

---

_Label `E-medium` added by @epage on 2024-08-10 00:16_

---

_Label `A-completion` added by @epage on 2024-08-10 00:16_

---

_Label `C-enhancement` added by @epage on 2024-08-10 02:09_

---

_Referenced in [clap-rs/clap#5706](../../clap-rs/clap/pulls/5706.md) on 2024-08-29 09:14_

---

_Comment by @shannmu on 2024-08-29 09:59_

This feature is undoubtedly a powerful capability. Let me try to explain my understanding of this feature.

For external subcommand completions, we can categorize them into cases that require forward completions and those that do not.

- For cases that do not require forward completions, such as `cargo <alias>`, this is simple to implement.

- For cases that require forward completions, such as `mycommand git [TAB]`, we need to address the following:

    For external subcommand completions, we might need to clarify the type of subcommand. I categorize them into external commands implemented by clap and those not implemented by clap:

    - **For external commands implemented by clap**: We could potentially save some metadata in a special path, which might allow us to achieve shell-independent completions within clap itself. We might call `COMPLETE=<shell> external_subcommand -- xx xx xx xx` to obtain completion results.

    - **For external commands not implemented by clap**: Their completions are necessarily shell-specific because we have no knowledge about how to complete them or what their completion scripts are like. We need to construct a new command-line input (excluding argv[0]) and then invoke some shell built-in commands (or write a script) to obtain completion results.
        - fish
        `complete --do-complete="<command line>"` could get the completion of `<command line>`
        
        - bash
        We could reset `COMP_LINE`, `COMP_WORDS`, `COMP_POINT`, `COMP_CWORD` to get completions from `COMPRELAY`
        
        - There are also some other shells for which I am currently unsure how to implement this feature.


---

_Label `E-medium` removed by @epage on 2024-08-29 17:55_

---

_Label `E-hard` added by @epage on 2024-08-29 17:55_

---

_Comment by @epage on 2024-08-29 17:57_

> For cases that do not require forward completions, such as cargo <alias>, this is simple to implement.

I view this as separate from this issue though it would be good to have a way to solve it.

> For external commands implemented by clap: 

imo users and other applications should not be aware an application is using `clap`.

> For external commands not implemented by clap:

This is likely what we'll need to do.

I put this as one of the lowest priority items.  I do not believe it is needed for parity with existing `clap_complete` or for `cargo`.

---

_Referenced in [clap-rs/clap#6107](../../clap-rs/clap/issues/6107.md) on 2025-08-19 13:34_

---

_Referenced in [clap-rs/clap#6016](../../clap-rs/clap/issues/6016.md) on 2025-11-03 21:59_

---

_Referenced in [clap-rs/clap#3266](../../clap-rs/clap/issues/3266.md) on 2025-11-03 21:59_

---
