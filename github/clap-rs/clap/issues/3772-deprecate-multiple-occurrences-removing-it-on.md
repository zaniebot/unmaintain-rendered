---
number: 3772
title: Deprecate multiple_occurrences, removing it on next breaking change
type: issue
state: closed
author: epage
labels:
  - M-breaking-change
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2022-05-31T14:48:59Z
updated_at: 2022-06-08T15:25:14Z
url: https://github.com/clap-rs/clap/issues/3772
synced_at: 2026-01-07T13:12:19-06:00
---

# Deprecate multiple_occurrences, removing it on next breaking change

---

_Issue opened by @epage on 2022-05-31 14:48_

With #3405, we are adding Actions.  This would allow us to fold multiple occurrences into Actions if we are willing for [Command::args_override_self](https://docs.rs/clap/latest/clap/struct.App.html#method.args_override_self) to be exclusively the behavior.

This would remove mixing of multiple occurrences and multiple values for positionals, leading us to reject issues like #3763.

This means we need to revisit #2993.  We'd need to have different behavior for flags (default to `Extend`) and positional arguments (default to `Set` with multiple values).

---

_Label `M-breaking-change` added by @epage on 2022-05-31 14:48_

---

_Label `S-waiting-on-decision` added by @epage on 2022-05-31 14:48_

---

_Label `A-parsing` added by @epage on 2022-05-31 14:48_

---

_Added to milestone `4.0` by @epage on 2022-05-31 14:48_

---

_Renamed from "Remove multiple_occurrences" to "Deprecate multiple_occurrences, removing it on next breaking change" by @epage on 2022-06-07 20:44_

---

_Referenced in [clap-rs/clap#3763](../../clap-rs/clap/issues/3763.md) on 2022-06-08 14:35_

---

_Closed by @epage on 2022-06-08 15:25_

---

_Referenced in [clap-rs/clap#3822](../../clap-rs/clap/issues/3822.md) on 2022-06-13 14:57_

---
