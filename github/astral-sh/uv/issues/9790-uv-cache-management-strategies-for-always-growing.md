---
number: 9790
title: uv cache management strategies for always-growing caches?
type: issue
state: open
author: charlesnicholson
labels:
  - enhancement
  - help wanted
  - cache
  - needs-design
assignees: []
created_at: 2024-12-10T22:00:08Z
updated_at: 2025-12-05T00:53:52Z
url: https://github.com/astral-sh/uv/issues/9790
synced_at: 2026-01-07T13:12:18-06:00
---

# uv cache management strategies for always-growing caches?

---

_Issue opened by @charlesnicholson on 2024-12-10 22:00_

Hello `uv` folks! Thanks again for making such an awesome tool; it's optimized and streamlined so many parts our Python workflow.

We have a helpful little CI robot that builds our code from scratch every week without using our constraints files, to get the latest versions of various third-party Python packages we depend on. It then freezes these constraints files and opens a PR that updates them in the repository. This technique allows us to go "back in time" and get a set of first- and third-party packages that were used at the time of that historical build.

This strategy works great, but because we're constantly upgrading to the latest versions of all of these packages, our local `uv` caches end up getting huge- mine was multiple GB when I last looked!

We can remember to clear the cache at regular intervals (or not, and just let it grow), but I was wondering if we're missing a workflow that would keep our cache sizes reasonable, or if there's a way to tell `uv` something persistent like:
* Only cache the latest version of a package by default
* Delete the earliest versions of packages when the cache exceeds a size threshold (LRU eviction)
* ?

Thanks for reading!

---

_Referenced in [astral-sh/uv#5731](../../astral-sh/uv/issues/5731.md) on 2024-12-29 20:33_

---

_Label `enhancement` added by @zanieb on 2025-01-07 19:12_

---

_Label `needs-design` added by @zanieb on 2025-01-07 19:12_

---

_Label `help wanted` added by @zanieb on 2025-01-07 19:12_

---

_Label `cache` added by @zanieb on 2025-01-07 19:13_

---

_Referenced in [astral-sh/uv#16761](../../astral-sh/uv/issues/16761.md) on 2025-11-17 15:29_

---

_Comment by @antonimmo on 2025-11-28 12:35_

I just came across this issue. For a single `uv sync` call, we gather around 1GB of data, I cannot image (`uv cache clean` is still running as I type this comment) how many GB we've gathered since March this year when I started to use `uv`.

I would set a default time to expire uv cache, something around 90 days would be sensible, imo.

Thanks!

---

_Comment by @davidgilbertson on 2025-12-05 00:53_

I started using `uv` a few months back, I'm up to 18GB already. A setting to cap this would be great.

---
