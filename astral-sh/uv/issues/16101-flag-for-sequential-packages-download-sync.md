```yaml
number: 16101
title: Flag for sequential packages download/sync
type: issue
state: closed
author: Khaled-Fawry
labels:
  - enhancement
assignees: []
created_at: 2025-10-02T10:38:12Z
updated_at: 2025-10-02T11:19:43Z
url: https://github.com/astral-sh/uv/issues/16101
synced_at: 2026-01-12T16:02:23Z
```

# Flag for sequential packages download/sync

---

_@Khaled-Fawry_

### Summary

UV Version: 0.8.22
The operating system: Debian GNU/Linux 13 (trixie)
Command Ran: `uv sync`

**Problem**
When I run uv sync with many packages on a bad internet connection, it constantly fails with this timeout error:

`Failed to download distribution due to network timeout. Try increasing
UV_HTTP_TIMEOUT (current value: 30s)`

This happens because uv is downloading a ton of packages in parallel. If one download stalls or fails, all its progress is lost. I get stuck in a painful loop: try to sync → fail → lose progress → try again → fail...

Increasing `UV_HTTP_TIMEOUT` doesn't always work; the network is just too unstable for many parallel streams.

**Suggested Fix**
Add a flag, maybe --sequential, to force uv to download one package at a time. This puts less strain on the connection and makes timeouts less likely.

### Example

`uv sync --sequential`

---

_Label `enhancement` added by @Khaled-Fawry on 2025-10-02 10:38_

---

_Comment by @konstin on 2025-10-02 10:41_

You can reduce the number of parallel downloads with [UV_CONCURRENT_DOWNLOADS](https://docs.astral.sh/uv/reference/environment/#uv_concurrent_downloads).

---

_Comment by @Khaled-Fawry on 2025-10-02 11:19_

Great! Thank you so much!

---

_Closed by @Khaled-Fawry on 2025-10-02 11:19_

---
