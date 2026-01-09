---
number: 5819
title: Add Subcommand help headings
type: pull_request
state: open
author: Fiedzia
labels: []
assignees: []
base: master
head: command-groups
created_at: 2024-11-14T23:22:11Z
updated_at: 2025-10-31T17:57:20Z
url: https://github.com/clap-rs/clap/pull/5819
synced_at: 2026-01-07T13:12:20-06:00
---

# Add Subcommand help headings

---

_Pull request opened by @Fiedzia on 2024-11-14 23:22_

Group subcommands displayed in help (https://github.com/clap-rs/clap/issues/1553)
This is for help only, does not affect matching.

I've done the builder part, still figuring out derive support
(ah - it works automatically by setting subcommand_help_header=Some("foo"). I'd probably should just document it somewhere).

This is done the same way grouping args works: subcommand has optional subcommand_help_heading.
If provided, it will be used to group commands visually when displaying subcommand help.

so that we can go from

Commands:
cmd1 ...
cmd2 ...
help ...

to

Commands:
help ...

Utility commands:
cmd1 ...
cmd2 ....

Nothing changes if groups aren't used. See tests for examples.

Issues:
having subcommand_heading and subcommand_help_heading may be confusing, maybe naming should change.

---

_@epage reviewed on 2024-11-15 22:46_

---

_Review comment by @epage on `clap_builder/src/builder/command.rs`:2314 on 2024-11-15 22:46_

This will also need a way to set the help heading for the current command

---

_@epage reviewed on 2024-11-15 22:47_

---

_Review comment by @epage on `clap_builder/src/builder/command.rs`:2314 on 2024-11-15 22:47_

This will likely run into name conflicts with `subcommand_help_heading`.  We should probably deprecate that and migrate people over to setting `next_subcommand_help_heading`.

---

_@epage reviewed on 2024-11-15 22:48_

---

_Review comment by @epage on `clap_builder/src/builder/command.rs`:2314 on 2024-11-15 22:48_

The derive has special handling for regular help headings, so we should probably have the same for these

---

_@epage reviewed on 2024-11-15 22:48_

---

_Review comment by @epage on `clap_builder/src/builder/command.rs`:2314 on 2024-11-15 22:48_

Should we only have one `next_help_heading`?

---

_@epage reviewed on 2024-11-15 22:49_

---

_Review comment by @epage on `tests/builder/subcommands.rs`:641 on 2024-11-15 22:49_

Before this can move forward, we need to come to a resolution on `help`.  The initial discussion should be on #1553.  If we need to make new issues off of that, then we can migrate from there.

---

_@Fiedzia reviewed on 2024-11-18 12:42_

---

_Review comment by @Fiedzia on `clap_builder/src/builder/command.rs`:2314 on 2024-11-18 12:42_

Good question, that would make api simpler, but I'd rather have clear separation between subcommand and parameter heading

---

_Review comment by @epage on `clap_builder/src/builder/command.rs`:2314 on 2024-11-18 16:43_

Could you explain why?

---

_@epage reviewed on 2024-11-18 16:43_

---

_@boozook approved on 2025-07-10 20:15_

---
