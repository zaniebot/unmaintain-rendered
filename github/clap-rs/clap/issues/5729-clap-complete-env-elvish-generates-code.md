---
number: 5729
title: "`clap_complete::env::Elvish` generates code deprecated in 0.18, removed in 0.21"
type: issue
state: open
author: epage
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2024-09-16T14:27:22Z
updated_at: 2024-09-16T14:27:23Z
url: https://github.com/clap-rs/clap/issues/5729
synced_at: 2026-01-07T13:12:20-06:00
---

# `clap_complete::env::Elvish` generates code deprecated in 0.18, removed in 0.21

---

_Issue opened by @epage on 2024-09-16 14:27_

As reported in rust-lang/cargo#14545
```
++++ actual:   In-memory
        1 + Deprecation: the legacy temporary assignment syntax is deprecated; use "tmp" instead
        2 +   /tmp/cargo/target/tmp/cit/t0/home/elvish/rc.elv:3:7: eval (E:CARGO_COMPLETE=elvish cargo | slurp)
```

From [0.18](https://elv.sh/blog/0.18.0-release-notes.html)

> The legacy temporary assignment syntax (e.g. a=foo echo $a) is deprecated. Use the new tmp command instead (e.g. tmp a = foo; echo $a).
> ...
> A new tmp special command for doing temporary assignments.

From [0.21](https://elv.sh/blog/0.21.0-release-notes.html)

> Support for the legacy temporary assignment syntax (a=b command), deprecated since 0.18.0, has been removed.
>
> Use either the [tmp](https://elv.sh/ref/language.html#tmp) command (available since 0.18.0) or the [with](https://elv.sh/ref/language.html#with) command (available since this release) instead.

Note: since I don't see this locally (0.17) or in CI, this means we need likely need a version check to determine which approach is allowed or find a different way of writing this completely

---

_Label `C-bug` added by @epage on 2024-09-16 14:27_

---

_Label `E-medium` added by @epage on 2024-09-16 14:27_

---

_Label `A-completion` added by @epage on 2024-09-16 14:27_

---

_Referenced in [clap-rs/clap#3166](../../clap-rs/clap/issues/3166.md) on 2024-09-16 14:27_

---

_Referenced in [clap-rs/clap#5869](../../clap-rs/clap/pulls/5869.md) on 2025-01-06 13:39_

---

_Referenced in [clap-rs/clap#6168](../../clap-rs/clap/issues/6168.md) on 2025-10-30 14:42_

---

_Referenced in [clap-rs/clap#5329](../../clap-rs/clap/issues/5329.md) on 2025-11-24 15:49_

---
