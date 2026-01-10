```yaml
number: 347
title: Add user feedback when building source distributions in the resolver
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/progress
created_at: 2023-11-06T22:41:09Z
updated_at: 2023-11-07T14:17:32Z
url: https://github.com/astral-sh/uv/pull/347
synced_at: 2026-01-10T15:50:28Z
```

# Add user feedback when building source distributions in the resolver

---

_Pull request opened by @charliermarsh on 2023-11-06 22:41_

It looks like Cargo, notice the bold green lines at the top (which appear during the resolution, to indicate Git fetches and source distribution builds):

<img width="868" alt="Screen Shot 2023-11-06 at 11 28 47 PM" src="https://github.com/astral-sh/puffin/assets/1309177/9647a480-7be7-41e9-b1d3-69faefd054ae">

<img width="868" alt="Screen Shot 2023-11-06 at 11 28 51 PM" src="https://github.com/astral-sh/puffin/assets/1309177/6bc491aa-5b51-4b37-9ee1-257f1bc1c049">

Closes https://github.com/astral-sh/puffin/issues/287 although we can do a lot more here.


---

_Comment by @charliermarsh on 2023-11-06 22:44_

Actually, I'm going to add Git fetches to this too.

---

_Comment by @charliermarsh on 2023-11-07 01:59_

Going absolutely insane trying to get indicatif to work here.

---

_Comment by @charliermarsh on 2023-11-07 04:20_

Okay, it now also communicates back to the user when we fetch / update a Git repo.

---

_Review requested from @zanieb by @charliermarsh on 2023-11-07 04:20_

---

_Review requested from @konstin by @charliermarsh on 2023-11-07 04:20_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/reporters.rs`:201 on 2023-11-07 09:20_

In my experience multiprogress glitches a lot, i think that's why cargo doesn't use it. I'd go with something like

```
Downloaded pydantic-code
Updated https://github.com/numpy/numpy.git (5e595f)
⠋ numpy (build), pydantic (git clone), torch (download 150/670MB), ...and 5 more
```
Though i also like the cargo way where it doesn't tell you what finished, but only what started.

---

_@konstin approved on 2023-11-07 09:25_

---

_@charliermarsh reviewed on 2023-11-07 14:11_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:201 on 2023-11-07 14:11_

The good news at least is this implementation entirely decouples the event layer from the reporting layer, so we can easily change the internals here.

---

_@charliermarsh reviewed on 2023-11-07 14:13_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/reporters.rs`:201 on 2023-11-07 14:13_

We could also look at Cargo's implementation because we're not _really_ using multi-progress here. We just have static text (that changes once) on top, followed by a single dynamic progress bar.

---

_Merged by @charliermarsh on 2023-11-07 14:17_

---

_Closed by @charliermarsh on 2023-11-07 14:17_

---

_Branch deleted on 2023-11-07 14:17_

---
