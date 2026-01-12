```yaml
number: 5337
title: "Respect `flatten_help` in man pages"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - S-waiting-on-design
  - A-man
assignees: []
created_at: 2024-02-02T18:42:37Z
updated_at: 2024-02-02T18:42:38Z
url: https://github.com/clap-rs/clap/issues/5337
synced_at: 2026-01-12T16:14:16Z
```

# Respect `flatten_help` in man pages

---

_@epage_

#5206 added support for subcommands like `git stash` that summarize all of their subcommands.  Unlike git, we show short help on `git stash --help` for these subcommands and long help on `git stash pop --help`.

The question is how these should be handled in man pages.

Example idea:
- Flattened subcommands have their long help embedded into their parent command's man page and `generate` skips generating files for these (see #5331)

---

_Label `C-enhancement` added by @epage on 2024-02-02 18:42_

---

_Label `S-waiting-on-design` added by @epage on 2024-02-02 18:42_

---

_Label `A-man` added by @epage on 2024-02-02 18:42_

---

_Referenced in [magic-wormhole/magic-wormhole.rs#233](../../magic-wormhole/magic-wormhole.rs/pulls/233.md) on 2024-07-06 17:24_

---

_Referenced in [JakeStanger/ironbar#683](../../JakeStanger/ironbar/pulls/683.md) on 2024-08-10 23:18_

---
