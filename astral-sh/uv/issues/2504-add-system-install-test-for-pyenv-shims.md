```yaml
number: 2504
title: Add system install test for pyenv shims
type: issue
state: open
author: charliermarsh
labels:
  - help wanted
  - testing
assignees: []
created_at: 2024-03-18T02:40:18Z
updated_at: 2024-07-01T21:58:05Z
url: https://github.com/astral-sh/uv/issues/2504
synced_at: 2026-01-10T05:31:37Z
```

# Add system install test for pyenv shims

---

_Issue opened by @charliermarsh on 2024-03-18 02:40_

_No description provided._

---

_Label `testing` added by @charliermarsh on 2024-03-18 02:40_

---

_Comment by @zanieb on 2024-03-18 14:38_

I started looking into this.... we probably want to avoid building from source each time but pyenv does not support linking a system Python version without a plugin (ref https://github.com/pyenv/pyenv/issues/1244).

We might want to:

1. Create a docker image with some pyenv Python versions prebuilt
2. Publish to GitHub's image registry (can we easily do this in this repository?)
3. Use that in the job
4. Add an optional assertion to our system check script that a given Python version is being used

---

_Comment by @CharString on 2024-04-04 21:23_

You can consider looking at https://github.com/jdx/mise or even use it. It already supports pyenv config files.

---

_Comment by @zanieb on 2024-04-04 22:02_

@CharString Sorry I'm not sure how that accomplishes our goal here? We want to make sure that people using `pyenv` have correct behavior with `uv`. The complexity here is avoiding build times.

---

_Comment by @CharString on 2024-04-05 01:09_

@zanieb Oh, sorry. I misunderstood the issue then. I thought this was the start of implementing pyenv functionality in uv. Which mise had already done.

---

_Label `help wanted` added by @zanieb on 2024-07-01 21:58_

---
