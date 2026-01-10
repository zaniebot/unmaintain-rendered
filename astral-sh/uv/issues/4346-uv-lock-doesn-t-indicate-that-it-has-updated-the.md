---
number: 4346
title: "`uv lock` doesn't indicate that it has updated the lock file"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - tracing
  - cli
  - preview
assignees: []
created_at: 2024-06-16T23:35:27Z
updated_at: 2024-07-17T01:14:21Z
url: https://github.com/astral-sh/uv/issues/4346
synced_at: 2026-01-10T01:23:37Z
---

# `uv lock` doesn't indicate that it has updated the lock file

---

_Issue opened by @zanieb on 2024-06-16 23:35_

The output is just "Resolved ... packages in ...", maybe we should indicate that we've updated the file or locked dependencies?

Even the verbose output doesn't show any indication of updating the lock file.

---

_Label `tracing` added by @zanieb on 2024-06-16 23:35_

---

_Label `preview` added by @zanieb on 2024-06-16 23:35_

---

_Comment by @zanieb on 2024-06-16 23:35_

It'd probably be nice to know if the file was left unchanged or if we updated it.

---

_Comment by @kidrahahjo on 2024-06-17 03:06_

I agree. Would it also make sense to give a brief description of what changed (it can be enabled by default and users would be able to disable it using a `--no-summary` flag)? 

The output command can be
|  package | old version  | new version  | action
|---|---|---|---|
| xyz |   | 1.0.0  | ADDED |
| abc  |  0.0.1 |   | DELETED |
|  efg |  0.1.0 | 0.1.1  | UPDATED  |

Although this has a downside of a huge output on initial lock creation (and probably more things). Apart from this, for a usual development workflow, teams can always check the diff to see what has changed.

---

_Comment by @zanieb on 2024-06-17 21:31_

Cargo has a nice example here

```

Updating lockfile...
    Updating uv v0.2.11 (/Users/zb/workspace/uv/crates/uv) -> v0.2.12
    Updating uv-version v0.2.11 (/Users/zb/workspace/uv/crates/uv-version) -> v0.2.12
note: pass `--verbose` to see 102 unchanged dependencies behind latest
```

---

_Comment by @kidrahahjo on 2024-06-18 00:47_

Yeah, this is a nice inspiration.

I see `cargo update --verbose` is way less verbose than `uv lock --verbose`. Do you think we should transition the current verbose flag to very verbose (-vv) and have the verbose flag (-v) for a pretty verbose output?

---

_Comment by @zanieb on 2024-06-18 01:25_

We have a tracking issue for that at https://github.com/astral-sh/uv/issues/1569, we need to do quite a bit of work around our tracing output levels eventually it's just not as high of a priority as the rest of the interface right now.

---

_Referenced in [astral-sh/uv#4854](../../astral-sh/uv/issues/4854.md) on 2024-07-07 16:55_

---

_Label `help wanted` added by @zanieb on 2024-07-07 16:55_

---

_Label `cli` added by @zanieb on 2024-07-07 16:55_

---

_Referenced in [astral-sh/uv#5110](../../astral-sh/uv/pulls/5110.md) on 2024-07-16 15:15_

---

_Closed by @charliermarsh on 2024-07-17 01:14_

---

_Closed by @charliermarsh on 2024-07-17 01:14_

---
