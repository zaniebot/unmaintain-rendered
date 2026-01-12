```yaml
number: 287
title: Add user-facing feedback when fetching Git dependencies
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-11-02T03:19:55Z
updated_at: 2023-11-07T14:17:33Z
url: https://github.com/astral-sh/uv/issues/287
synced_at: 2026-01-12T15:58:22Z
```

# Add user-facing feedback when fetching Git dependencies

---

_@charliermarsh_

_No description provided._

---

_Label `enhancement` added by @charliermarsh on 2023-11-02 03:19_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-02 03:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-02 20:11_

---

_Comment by @charliermarsh on 2023-11-04 15:39_

Poetry does this which is handy (effectively, one progress entry per package):

<img width="1792" alt="Screen Shot 2023-11-04 at 11 38 05 AM" src="https://github.com/astral-sh/puffin/assets/1309177/8b2a2f50-63e7-4195-9eab-e31113ba7046">

They don't show any feedback for the resolver, I'm considering showing an ephemeral progress bar for every package we need to build... I'd love input or ideas though.


---

_Comment by @zanieb on 2023-11-04 15:42_

Ephemeral progress sounds nice, either a little window into logs or a bar?

---

_Comment by @charliermarsh on 2023-11-04 15:42_

Cargo has a global progress bar but puts operations above it. It also tells you if it needs to update a repository:

<img width="1792" alt="Screen Shot 2023-11-04 at 11 41 49 AM" src="https://github.com/astral-sh/puffin/assets/1309177/1a9e2e7d-bc2a-4ab9-986e-ba73f084ce1e">


---

_Comment by @charliermarsh on 2023-11-04 15:45_

I guess there's also a question of what we want to show during resolution vs. installation. Installation is easier because we know everything we need to do upfront.

---

_Comment by @charliermarsh on 2023-11-04 15:46_

I was playing with this basic concept (ignore the actual styling):

<img width="836" alt="Screen Shot 2023-11-04 at 11 45 14 AM" src="https://github.com/astral-sh/puffin/assets/1309177/8f7e5280-7311-4974-be5a-d3ce65eadd7e">

<img width="836" alt="Screen Shot 2023-11-04 at 11 45 16 AM" src="https://github.com/astral-sh/puffin/assets/1309177/ade3a6eb-9ca5-4ec4-970f-49a8a99a121a">


---

_Unassigned @charliermarsh by @charliermarsh on 2023-11-04 15:47_

---

_Comment by @charliermarsh on 2023-11-04 15:49_

Un-assigning because I'm working on correctness things before this. But the basic approach is visible in https://github.com/astral-sh/puffin/compare/charlie/progress?expand=1. We'd also need to thread a similar reporter abstraction through to `puffin-git`. (Cargo does this with a global `Config` thing that exposes a `shell()` method. I don't know that we need that right now.)

---

_Comment by @konstin on 2023-11-05 13:04_

If possible, i'd prefer only a single line changing, optionally with past information above (like cargo), multiline progress bars glitch often.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-06 21:06_

---

_Comment by @charliermarsh on 2023-11-06 21:06_

Gonna add something simple here to at least wire up the data.

---

_Closed by @charliermarsh on 2023-11-07 14:17_

---
