---
number: 7273
title: Nexus PyPi causing early exit on package resolution
type: issue
state: closed
author: JTCroft
labels:
  - question
assignees: []
created_at: 2024-09-10T19:58:41Z
updated_at: 2024-09-12T19:04:24Z
url: https://github.com/astral-sh/uv/issues/7273
synced_at: 2026-01-10T01:24:12Z
---

# Nexus PyPi causing early exit on package resolution

---

_Issue opened by @JTCroft on 2024-09-10 19:58_

I have an issue related to an internal nexus-hosted PyPi. When using a repo group in nexus the page returned when a package is NOT present in any of the PyPis grouped together in a repo group will cause `uv` to believe that the package WAS found in the repo but with no versions available, invariably causing it to fail to install anything hosted in any package index after the internal nexus one in resolution order

Unlike nexus repos, using repo groups means that when a package isn’t found it returns a page with `Links to <package>` and then no links below.

Would it be possible for uv to treat an empty list of files as the package not being found in the index and continue through any remaining extra-index-urls and the index-url?

this was with uv 0.4.8, caused by running `uv pip install <exernal package>` with a nexus repo group included in a uv.toml `extra-index-url = [“<internal nexus repo group>”]`

---

_Comment by @zanieb on 2024-09-10 20:09_

Have you tried changing the index strategy? https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes

---

_Label `question` added by @zanieb on 2024-09-10 22:29_

---

_Comment by @JTCroft on 2024-09-12 17:37_

Thanks for suggesting that, it’s a much better solution

Happy this to be closed if you don’t think it is a uv bug since it’s kind of a nexus issue - but other nexus users may well see this behaviour where packages that don’t exist in the index will be ‘found’

---

_Comment by @zanieb on 2024-09-12 19:04_

Thanks!

---

_Closed by @zanieb on 2024-09-12 19:04_

---
