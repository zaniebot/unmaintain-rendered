---
number: 13596
title: Compiling bytecode can significantly increase Docker image size
type: issue
state: closed
author: maxfirman
labels: []
assignees: []
created_at: 2025-05-22T15:37:25Z
updated_at: 2025-05-22T18:24:41Z
url: https://github.com/astral-sh/uv/issues/13596
synced_at: 2026-01-10T01:25:35Z
---

# Compiling bytecode can significantly increase Docker image size

---

_Issue opened by @maxfirman on 2025-05-22 15:37_

I had a Docker image that went from 750MB to 900MB with the `UV_COMPILE_BYTECODE=1` set. 

It would be worthwile highlighting that the image size can increase in the docs 
 [here](https://docs.astral.sh/uv/guides/integration/docker/#compiling-bytecode).

---

_Comment by @konstin on 2025-05-22 16:55_

I think it's reasonable to expect that ahead of time (bytecode) compilation increases the artifact size. The compilation is trade-off for a slower install with larger outputs for a faster startup. This isn't uv specific, I don't think we need to have this called out in the docs.

---

_Closed by @konstin on 2025-05-22 16:55_

---

_Comment by @zanieb on 2025-05-22 18:24_

Yeah you're trading increased image size for faster start-up time. I'm not sure if we need to explain that explicitly.

---
