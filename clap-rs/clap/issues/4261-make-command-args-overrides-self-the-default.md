```yaml
number: 4261
title: "Make `Command::args_overrides_self` the default"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2022-09-26T18:08:29Z
updated_at: 2022-09-26T18:37:15Z
url: https://github.com/clap-rs/clap/issues/4261
synced_at: 2026-01-12T16:14:15Z
```

# Make `Command::args_overrides_self` the default

---

_@epage_

We had [talked about swapping the default for 3.0](https://github.com/clap-rs/clap/discussions/2627) but punted.

When working on `ArgAction` during 3.2, I modeled it after Python's `argparse` which only supports overriding, making `args_overrides_self` the only behavior for the new actions.

When [switching cargo to the new actions](https://github.com/rust-lang/cargo/pull/11117), some tests specifically checking for self-conflicts failed.  This reminded me that in [cargo 1.64.0](https://github.com/rust-lang/rust/blob/master/RELEASES.md#version-1640-2022-09-22), `--target` switched from a `multiple_occurrences(false)` to `multiple_occurrences(true)`.  This was a switch from an error case to a working case.  If `args_overrides_self` had been on, it would have been a switch from a working case to a working case, being a change in working behavior.

[We decided](https://rust-lang.zulipchat.com/#narrow/stream/246057-t-cargo/topic/Behavior.20Change.20with.20Clap.20v4.20Upgrade) in 4.0.0 to defer a decision on the behavior and bring back `args_overrides_self`, making the new actions behave like the old ones,

So to summarize `args_overrides_self(true)`
- Allows easy overriding of previous arguments, especially if args/aliases are involved
  - e.g. `alias ls='ls -xyz'` would be safe to repeat some of those flagd
- Has compatibility hazards

So should we switch to it being on by default, only on, or maintain status quo?

Notes
- Overriding of self is the exclusive behavior for Python's `argparse`
- [A user on Zulip thought `args_overrides_self(true)` was set](https://rust-lang.zulipchat.com/#narrow/stream/246057-t-cargo/topic/Behavior.20Change.20with.20Clap.20v4.20Upgrade/near/300447239)
- [A user reported the conflict error as a bug](https://bugzilla.redhat.com/show_bug.cgi?id=1511821)
- Most tools Josh has used seem to ignore duplicate flags, at least (e.g. `rm -fffff file`).
- And at least some tools silently override option values as well; for instance, `touch -d 'Jan 1 1970' -d 'Dec 31 2400' file` will use the latter timestamp.

---

_Label `C-enhancement` added by @epage on 2022-09-26 18:08_

---

_Label `S-waiting-on-decision` added by @epage on 2022-09-26 18:08_

---

_Label `A-parsing` added by @epage on 2022-09-26 18:08_

---

_Referenced in [clap-rs/clap#4262](../../clap-rs/clap/pulls/4262.md) on 2022-09-26 18:30_

---

_Label `M-breaking-change` added by @epage on 2022-09-26 18:37_

---

_Referenced in [clap-rs/clap#4281](../../clap-rs/clap/pulls/4281.md) on 2022-09-28 21:10_

---

_Referenced in [astral-sh/uv#3248](../../astral-sh/uv/issues/3248.md) on 2024-04-25 02:54_

---
