```yaml
number: 411
title: Add support for a JSON output format for machine readable output throughout uv
type: issue
state: open
author: konstin
labels:
  - enhancement
  - help wanted
  - cli
  - tracking
assignees: []
created_at: 2023-11-13T13:26:35Z
updated_at: 2025-12-02T09:50:12Z
url: https://github.com/astral-sh/uv/issues/411
synced_at: 2026-01-10T03:23:52Z
```

# Add support for a JSON output format for machine readable output throughout uv

---

_Issue opened by @konstin on 2023-11-13 13:26_

Our main commands should have a json output format that can be in downstream applications such as auditing, dependabot-like application and our own testing.

See the linked issues for support in a specific command.

---

_Label `enhancement` added by @konstin on 2023-11-13 13:26_

---

_Renamed from "output format json" to "Output format json" by @konstin on 2023-11-13 13:49_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-13 14:40_

---

_Label `internal` added by @charliermarsh on 2023-11-13 14:40_

---

_Comment by @bigluck on 2024-02-15 21:32_

I'd love it, too :)

---

_Comment by @zanieb on 2024-07-01 21:30_

https://github.com/astral-sh/uv/issues/3199 has relevant context for implementing this in the `uv pip` interfaces

---

_Label `internal` removed by @zanieb on 2024-07-01 21:30_

---

_Label `help wanted` added by @zanieb on 2024-07-01 21:30_

---

_Label `cli` added by @zanieb on 2024-07-01 21:30_

---

_Comment by @robinjhuang on 2025-01-31 23:59_

Would love to have this for non-interactive terminals! Any idea when this will be implemented? @Gankra 

---

_Comment by @Gankra on 2025-02-21 17:33_

This is gonna take a while and will have to be done piecemeal interface-by-interface.

Some commands like `uv python list` will probably want to just dump a single json object.

Other commands that compile/install packages will probably benefit from a json-lines streaming output, like [the one you can see in cargo/rustc's interface](https://doc.rust-lang.org/cargo/reference/external-tools.html#json-messages).

It would be helpful to know what commands people specifically need/want this output from to prioritize.

---

_Comment by @edgarrmondragon on 2025-02-21 17:52_

> It would be helpful to know what commands people specifically need/want this output from to prioritize.

https://github.com/astral-sh/uv/issues/1442 would be at the top of my list 🙂 

---

_Comment by @robinjhuang on 2025-03-08 04:15_

`uv pip install` is at the top of my list! When installing large downloads like pytorch, it's really helpful to have some form of machine readable progress. 

https://github.com/astral-sh/uv/issues/11121

---

_Comment by @robinjhuang on 2025-05-06 06:40_

Is this still planned?

---

_Comment by @zanieb on 2025-05-16 15:22_

Yep!

---

_Renamed from "Output format json" to "Add support for a JSON output format for machine readable output throughout uv" by @zanieb on 2025-06-13 12:03_

---

_Label `tracking` added by @zanieb on 2025-06-13 12:06_

---

_Comment by @InfiniteRain on 2025-08-06 13:07_

If breaking json schemas too often is a concern, perhaps versioning could be added. Stuff like `uv python list --format=json@v2`, this way breaking changes could be avoided.

---

_Removed from milestone `Future` by @zanieb on 2025-12-02 09:50_

---
