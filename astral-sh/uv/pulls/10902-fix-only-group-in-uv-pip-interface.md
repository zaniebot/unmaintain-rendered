```yaml
number: 10902
title: "fix `--only-group` in `uv pip` interface"
type: pull_request
state: merged
author: Gankra
labels:
  - bug
  - internal
assignees: []
merged: true
base: main
head: gankra/only-group
created_at: 2025-01-23T15:28:25Z
updated_at: 2025-01-23T16:35:50Z
url: https://github.com/astral-sh/uv/pull/10902
synced_at: 2026-01-12T16:09:34Z
```

# fix `--only-group` in `uv pip` interface

---

_@Gankra_

This was an oversight in the implementation, thankfully it appears to be a simple fix? (My only hesitation is this implementation essentially claims that --only-group is defacto incompatible with --extra and I *think* that's the case but I'm not certain.)

---

_Label `bug` added by @Gankra on 2025-01-23 15:28_

---

_Label `internal` added by @Gankra on 2025-01-23 15:28_

---

_Review requested from @zanieb by @Gankra on 2025-01-23 15:28_

---

_Renamed from "fix --only-group in pip interface" to "fix `--only-group` in `uv pip` interface" by @Gankra on 2025-01-23 15:29_

---

_Comment by @Gankra on 2025-01-23 15:31_

Notably this *does not* resolve the fact that the upstream pip PR (https://github.com/pypa/pip/pull/13065) for this functionality uses `--group` for what we mean by `--only-group` (they have no equivalent for our `--group` and probably don't want it).

---

_Comment by @zanieb on 2025-01-23 15:52_

>  essentially claims that --only-group is defacto incompatible with --extra

Yes these are incompatible.

---

_@zanieb approved on 2025-01-23 15:57_

---

_Comment by @zanieb on 2025-01-23 15:59_

> > essentially claims that --only-group is defacto incompatible with --extra
> 
> Yes these are incompatible.

How does this manifest? Should we fail in the CLI with `conflicts_with`?

---

_Comment by @Gankra on 2025-01-23 16:15_

> How does this manifest? Should we fail in the CLI with conflicts_with?

I don't think anything particularly special happens it's just that the implications of --extra are meaningless once you apply --only-group. I imagine this is true with *many* other non-group flags and --only-group so I'm not sure it's worth especially calling out (it only stood out to me because I've been using --extra as kinda the guiding light for how to thread --group stuff through).

(I can't find any other part of the codebase where we do anything about this, in a quick skim.)

---

_Merged by @Gankra on 2025-01-23 16:35_

---

_Closed by @Gankra on 2025-01-23 16:35_

---

_Branch deleted on 2025-01-23 16:35_

---
