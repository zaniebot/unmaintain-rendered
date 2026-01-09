---
number: 15899
title: Add --offline-first flag to uvx to request from index if necessary but otherwise use local cache
type: issue
state: closed
author: raoulj
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2025-09-16T20:32:43Z
updated_at: 2025-09-16T20:48:21Z
url: https://github.com/astral-sh/uv/issues/15899
synced_at: 2026-01-07T13:12:19-06:00
---

# Add --offline-first flag to uvx to request from index if necessary but otherwise use local cache

---

_Issue opened by @raoulj on 2025-09-16 20:32_

### Summary

I have uvx commands I'm running in cron as a production system.  I want the command I'm running to be the minimal code necessary to run a script:
```cron
0 0 * * * /path/to/uvx --index private_index somepackge@1.2.3 some_script arg1
```
I love that updating production can be as simple as updating the version number.  A breath of fresh air.  Simple.

But I found that if the index is down, uvx hangs.  I want to remove the dependency on my internal index being up in order for production to run.  I can use --offline but if the cache is empty or cleared (which could happen on this system if the user uses uv and clears the cache) it'll break prod! 

So I ended up making a little script I call `uvx-resilient` which is:
```bash

#!/bin/bash
# uvx-resilient
# This exists because there's no way to read the cache first, else hit the index

UV=$HOME/.local/bin/uv

# Extract uv args
uv_args=()
while [[ $# -gt 0 ]] && [[ "$1" != *@* ]] || [[ "$1" == --* ]]; do
    uv_args+=("$1")
    shift
done

tool="$1"
shift

# Extract package and version
pkg=$(echo "$tool" | cut -d'@' -f1 | cut -d'[' -f1)
version=$(echo "$tool" | cut -d'@' -f2)

# Check if tool is not installed with correct version
if ! $UV tool list | grep -q "^$pkg v$version$"; then
    $UV tool install "${uv_args[@]}" --force "$tool" || exit 1
fi

exec $UV tool run --offline "${uv_args[@]}" --from "$tool" "$@"

```

I figure others may be self-hosting their indexes and want to make this all more resilient.  Thanks for creating uv.  Great tool.

### Example

_No response_

---

_Label `enhancement` added by @raoulj on 2025-09-16 20:32_

---

_Renamed from "Add --offline-first flag to uvx to request from index if necessary but otherwise using local cache" to "Add --offline-first flag to uvx to request from index if necessary but otherwise use local cache" by @raoulj on 2025-09-16 20:38_

---

_Comment by @zanieb on 2025-09-16 20:45_

Thanks for the details, this is a duplicate of https://github.com/astral-sh/uv/issues/10380 though

---

_Label `duplicate` added by @zanieb on 2025-09-16 20:45_

---

_Comment by @raoulj on 2025-09-16 20:48_

Apologies!  Didn't see that.

---

_Closed by @raoulj on 2025-09-16 20:48_

---
