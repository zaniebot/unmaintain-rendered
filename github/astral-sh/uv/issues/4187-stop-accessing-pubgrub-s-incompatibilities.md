---
number: 4187
title: "Stop accessing pubgrub's incompatibilities directly"
type: issue
state: open
author: konstin
labels:
  - internal
assignees: []
created_at: 2024-06-10T06:34:18Z
updated_at: 2024-06-10T12:54:00Z
url: https://github.com/astral-sh/uv/issues/4187
synced_at: 2026-01-07T13:12:17-06:00
---

# Stop accessing pubgrub's incompatibilities directly

---

_Issue opened by @konstin on 2024-06-10 06:34_

When computing the dependency edges, we currently use pubgrub's incompatibilities database:

https://github.com/astral-sh/uv/blob/e7c573cfcb069cc940b8838ecb5f8d1e4858c304/crates/uv-resolver/src/resolver/mod.rs#L1515-L1524

The incompatibilities are an implementation detail of pubgrub. We should instead use the requirements to construct the edges.

---

_Label `internal` added by @konstin on 2024-06-10 06:34_

---

_Comment by @charliermarsh on 2024-06-10 12:53_

I'm mixed on this though I understand the motivation. It seems natural that we should get the resolution from PubGrub given that it solved the graph!

---

_Referenced in [pubgrub-rs/pubgrub#233](../../pubgrub-rs/pubgrub/issues/233.md) on 2024-06-10 13:37_

---
