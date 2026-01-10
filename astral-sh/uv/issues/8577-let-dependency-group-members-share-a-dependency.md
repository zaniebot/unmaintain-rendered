---
number: 8577
title: Let dependency-group members share a dependency source
type: issue
state: open
author: ealap
labels:
  - needs-design
assignees: []
created_at: 2024-10-25T21:39:07Z
updated_at: 2024-10-25T22:00:07Z
url: https://github.com/astral-sh/uv/issues/8577
synced_at: 2026-01-10T01:24:30Z
---

# Let dependency-group members share a dependency source

---

_Issue opened by @ealap on 2024-10-25 21:39_

I have just read the recent release that added supported for PEP-735 and immediately went on to try it on my project. So far I have something like this:
```toml
[dependency-groups]
groupA = [
  "pytest",
  "ruff"
]
groupB = [
  "cool-stuff-1",
  "cool-stuff-2",
  "cool-stuff-3",
  "cool-stuff-4",
  "cool-stuff-5",
  "cool-stuff-6",
  "cool-stuff-7",
  "cool-stuff-8",
  "cool-stuff-9",
  "cool-stuff-10"
]
groupMACOS = [
  "apple-stuff-1",
  "apple-stuff-2",
  "apple-stuff-3"
]

[tool.uv.sources]
pytest = { index = "groupA" }
cool-stuff-1 = { index = "groupB" }
cool-stuff-2 = { index = "groupB" }
cool-stuff-3 = { index = "groupB" }
cool-stuff-4 = { index = "groupB" }
cool-stuff-5 = { index = "groupB" }
cool-stuff-6 = { index = "groupB" }
cool-stuff-7 = { index = "groupB" }
cool-stuff-8 = { index = "groupB" }
cool-stuff-9 = { index = "groupB" }
cool-stuff-10 = { index = "groupB" }
apple-stuff-1 = { index = "groupA", marker = "sys_platform == 'darwin'" }
apple-stuff-2 = { index = "groupA", marker = "sys_platform == 'darwin'" }
apple-stuff-3 = { index = "groupA", marker = "sys_platform == 'darwin'" }

[[tool.uv.index]]
name = "groupA"
url = "https://download.mypypi.org/whl/group/A"

[[tool.uv.index]]
name = "groupB"
url = "https://download.mypypi.org/whl/group/B"
```

As you can see, it's easy to make a wall of similar text when you need specific sources and specifiers for a couple or more dependencies. But most are just sharing the same 2 unique values: `{ index = "groupB" }`, `{ index = "groupA", marker = "sys_platform == 'darwin'" }`

To simplify this, it seems that next improvement is to have a group share a `sources` setting.
```toml
[dependency-groups]
groupA = [
  "pytest",
  "ruff"
]
groupB = [
  "cool-stuff-1",
  "cool-stuff-2",
  "cool-stuff-3",
  "cool-stuff-4",
  "cool-stuff-5",
  "cool-stuff-6",
  "cool-stuff-7",
  "cool-stuff-8",
  "cool-stuff-9",
  "cool-stuff-10"
]
groupMACOS = [
  "apple-stuff-1",
  "apple-stuff-2",
  "apple-stuff-3"
]

[tool.uv.sources]
pytest = { index = "groupA" }
groupB = { index = "groupB" }
groupMACOS = { index = "groupA", marker = "sys_platform == 'darwin'" }

[[tool.uv.index]]
name = "groupA"
url = "https://download.mypypi.org/whl/group/A"

[[tool.uv.index]]
name = "groupB"
url = "https://download.mypypi.org/whl/group/B"
```


---

_Comment by @zanieb on 2024-10-25 22:00_

I believe this is pretty similar to https://github.com/astral-sh/uv/issues/8415

We could provide a way to define sources for entire group.

---

_Label `needs-design` added by @zanieb on 2024-10-25 22:00_

---
