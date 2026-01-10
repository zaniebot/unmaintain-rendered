---
number: 17250
title: "Bump `taiki-e/install-action` to a version that supports `nextest` 0.9.115"
type: issue
state: open
author: EliteTK
labels: []
assignees: []
created_at: 2025-12-29T13:13:03Z
updated_at: 2025-12-29T13:13:24Z
url: https://github.com/astral-sh/uv/issues/17250
synced_at: 2026-01-10T01:26:15Z
---

# Bump `taiki-e/install-action` to a version that supports `nextest` 0.9.115

---

_Issue opened by @EliteTK on 2025-12-29 13:13_

### Summary

Currently our CI uses `nextest` 0.9.101 which does not support [profile inheritance](https://nexte.st/docs/configuration/#profile-inheritance).

It would be nice to make the [CI specific profiles](https://github.com/astral-sh/uv/pull/17220) inherit from each other to reduce duplication.

To update to `nextest` 0.9.115 we would need to update `taiki-e/install-action` to 2.63.3 (`d850aa816998e5cf15f67a78c7b933f2a5033f8a`) or later. There are quite a few changes and I am not personally familiar enough (yet...) with how GitHub actions work to really understand the impact from updating or how to effectively audit these changes.

Also some kind of audit of changes between `nextest` 0.9.101 and `nextest` 0.9.115 would need to be performed.

---
