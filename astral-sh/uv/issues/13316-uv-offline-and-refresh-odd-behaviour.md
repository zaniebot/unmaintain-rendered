---
number: 13316
title: UV_OFFLINE and --refresh odd behaviour
type: issue
state: closed
author: RomainBrault
labels:
  - bug
  - cli
assignees: []
created_at: 2025-05-06T16:25:13Z
updated_at: 2026-01-06T20:43:06Z
url: https://github.com/astral-sh/uv/issues/13316
synced_at: 2026-01-10T01:25:31Z
---

# UV_OFFLINE and --refresh odd behaviour

---

_Issue opened by @RomainBrault on 2025-05-06 16:25_

### Summary

While the flags follow the expected behavior, using the environment variables do not:


```bash
❯ UV_OFFLINE=0 UV_REFRESH=1 uv run python -c ''

❯ UV_OFFLINE=1 UV_REFRESH=1 uv run python -c ''

❯ UV_OFFLINE=0 uv run --refresh python -c ''
error: the argument '--refresh' cannot be used with '--offline'

❯ UV_OFFLINE=1 uv run --refresh python -c ''
error: the argument '--refresh' cannot be used with '--offline'

Usage: uv run --refresh

For more information, try '--help'.

❯ UV_OFFLINE="" UV_REFRESH="" uv run python -c ''
error: invalid value '' for '--offline': value was not a boolean

For more information, try '--help'.

❯ UV_OFFLINE="hello" UV_REFRESH="world" uv run python -c ''
error: invalid value 'hello' for '--offline': value was not a boolean

For more information, try '--help'.
```

Or maybe I am using wrong the "boolean flags" but I think in this case it would be nice to specify what are the accepted values for the boolean flags.

Otherwise I would expect `UV_OFFLINE=1 UV_REFRESH=1 uv run python -c ''` to fail (now it seems to pass) and `UV_OFFLINE=0 uv run --refresh python -c ''` should pass.

### Platform

Ubuntu 25.04

### Version

0.7.2

### Python version

_No response_

---

_Label `bug` added by @RomainBrault on 2025-05-06 16:25_

---

_Comment by @zanieb on 2025-05-06 16:37_

> error: invalid value '' for '--offline': value was not a boolean

I think this is reasonable.

> error: the argument '--refresh' cannot be used with '--offline'

This is enforced at the CLI level, before values are parsed. We'd have to move `--refresh` / `--offline` conflict enforcement out of Clap to fix this, which seems reasonable to do.

> UV_OFFLINE=1 UV_REFRESH=1 uv run python -c ''

`UV_REFRESH` does not exist, hence the success here.

---

_Label `cli` added by @zanieb on 2025-05-06 16:37_

---

_Comment by @RomainBrault on 2025-05-06 16:47_

Thanks, sorry the empty values and hello, world values. They where just here for reference, I think this is very reasonable indeed :)

---

_Comment by @RomainBrault on 2025-05-06 16:58_

Wouldn't a UV_REFRESH be reasonable too ?

I would like to use refresh when I have internet. If I don't have internet I can toggle UV_OFFLINE=1 to avoid errors. However all my commands containing --refresh would fail. It would be nice to be able to switch it off globally with a UV_REFRESH.

I can open another feature issue if necessary.

---

_Renamed from "UV_OFFLINE and UV_REFRESH odd behaviour" to "UV_OFFLINE and --refresh odd behaviour" by @RomainBrault on 2025-05-06 16:59_

---

_Comment by @zanieb on 2025-05-06 17:01_

Why do you want to force refresh the cache when you have internet? (In general, it's reasonable for us to add variables for flags, but I'm curious about the use-case)

You may find https://github.com/astral-sh/uv/issues/10380 interesting too.

---

_Comment by @RomainBrault on 2025-05-07 07:52_

I'm using uv in a test script (justfile). I install local dependencies (the one to test). I want to be sure to have the latest update of my local package (source and metadata) when run the test so I add the --refresh. The performance impact is low and guarantee a clean cache. However when I'm moving and don't have internet access I still wanna be able to test my code hence the UV_OFFLINE toggle.

---

_Referenced in [astral-sh/uv#13385](../../astral-sh/uv/issues/13385.md) on 2025-05-12 23:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2026-01-04 21:00_

---

_Referenced in [astral-sh/uv#17321](../../astral-sh/uv/pulls/17321.md) on 2026-01-05 00:22_

---

_Closed by @charliermarsh on 2026-01-06 20:43_

---
