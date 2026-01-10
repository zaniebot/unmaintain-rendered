---
number: 3881
title: "modify existing subcommands in `Command` struct"
type: issue
state: closed
author: emersonford
labels:
  - C-enhancement
assignees: []
created_at: 2022-06-28T22:43:13Z
updated_at: 2022-07-15T16:19:18Z
url: https://github.com/clap-rs/clap/issues/3881
synced_at: 2026-01-10T01:27:48Z
---

# modify existing subcommands in `Command` struct

---

_Issue opened by @emersonford on 2022-06-28 22:43_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.6

### Describe your use case

I'm writing a Clap CLI that will eventually replace an existing Python CLI we have. The goal is to have this new CLI be the default one used and have it support fallback to the Python CLI for subcommands that are not yet implemented. Ideally, I'd like to *dynamically* generate the fallback subcommands as to not require any code changes / manually syncing between the Python version and the Clap/Rust version.

While `.allow_external_subcommands` gets me sort of there, but it gets messy in this instance:
1. Python CLI has command `foo generate bar` and `foo read bar`
2. New Clap CLI has command `foo generate baz` and `foo read baz` but not `foo generate bar` or `foo read bar`.

Currently, we'd have to add `.allow_external_subcommands` to both the `generate` subcommand and `read` subcommand, and implement duplicate matching logic for both. This also can't be done dynamically and we'd have to update the Rust/Clap CLI if there are any changes to the Python CLI.

What I'd ideally like is, traverse an existing `Command` we have (i.e. made with the Derive API) and insert subcommands found from the Python CLI command tree (which I can already generate) that are not in our current `Command` struct. This part is currently not possible as there's no way to modify existing subcommands as far as I'm aware. I can then have logic with `get_matches()` to detect when a fallback subcommand is used.



### Describe the solution you'd like

There's already an API to modify existing args, `mut_arg` so I think the best solution would just be to duplicate this logic in a `mut_subcommand` method.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @emersonford on 2022-06-28 22:43_

---

_Referenced in [clap-rs/clap#3882](../../clap-rs/clap/pulls/3882.md) on 2022-06-28 22:43_

---

_Comment by @epage on 2022-06-30 01:28_

Wow, that is a fairly limited solution, one that I would not normally prioritize for implementation but the tool proposed for the solution is fully aligned with what we would want anyways, so I'm ok with this moving forward.

---

_Closed by @emersonford on 2022-07-15 16:19_

---
