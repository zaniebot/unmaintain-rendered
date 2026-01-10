---
number: 3360
title: "universal-lock: implement merging of forked resolver states"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-03T18:18:24Z
updated_at: 2024-05-30T18:23:16Z
url: https://github.com/astral-sh/uv/issues/3360
synced_at: 2026-01-10T01:23:27Z
---

# universal-lock: implement merging of forked resolver states

---

_Issue opened by @BurntSushi on 2024-05-03 18:18_

As part of #3350, when generating a universal lock file, it's possible for the resolver to fork when there are conflicting dependencies. Once all forks have completed, the resolutions from each fork need to be merged into one resolution (which might have multiple versions of the same package). And that in turn needs to get converted to a `ResolutionGraph`.

In my prototype, this is where resolver states are unioned:

https://github.com/astral-sh/uv/blob/7f2b401260e6231bebb867c034b27d2955210fe0/crates/uv-resolver/src/resolver/mod.rs#L1367-L1484

And the resolution graph constructor takes a merged resolution as input:

https://github.com/astral-sh/uv/blob/7f2b401260e6231bebb867c034b27d2955210fe0/crates/uv-resolver/src/resolution.rs#L71-L78

---

_Label `preview` added by @BurntSushi on 2024-05-03 18:18_

---

_Referenced in [astral-sh/uv#3350](../../astral-sh/uv/issues/3350.md) on 2024-05-03 18:18_

---

_Referenced in [astral-sh/uv#3831](../../astral-sh/uv/pulls/3831.md) on 2024-05-24 19:04_

---

_Closed by @BurntSushi on 2024-05-30 18:23_

---
