```yaml
number: 6670
title: "Support `uv pip install -r uv.lock`"
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2024-08-27T02:45:40Z
updated_at: 2024-08-29T17:46:44Z
url: https://github.com/astral-sh/uv/issues/6670
synced_at: 2026-01-12T15:59:06Z
```

# Support `uv pip install -r uv.lock`

---

_@charliermarsh_

_No description provided._

---

_Label `needs-decision` added by @charliermarsh on 2024-08-27 02:45_

---

_Renamed from "Support `uv pip install -r uv.lock`?" to "Support `uv pip install -r uv.lock`" by @zanieb on 2024-08-27 11:16_

---

_Comment by @leddy231 on 2024-08-27 11:46_

I think this makes a lot of sense, this would allow installing packages from one or more lockfiles directly to the system python, without removing any packages (which `uv sync` would do). We need this for painless switch from poetry to uv :smile: 

---

_Comment by @charliermarsh on 2024-08-27 12:52_

I'm currently more in favor of adding `uv export` to `requirements.txt`, then `uv pip install -r requirements.txt`, since it's more flexible and composable.

---

_Comment by @leddy231 on 2024-08-27 13:00_

That would work too, and would be a clearer separation between project tools `uv ...` and raw pip tools.

Could they be piped together? A `uv export | uv pip install --system` oneliner in the dockerfile would be nice, instead of a intermediate file

---

_Comment by @shaunhegarty on 2024-08-27 17:17_

> That would work too, and would be a clearer separation between project tools `uv ...` and raw pip tools.
> 
> Could they be piped together? A `uv export | uv pip install --system` oneliner in the dockerfile would be nice, instead of a intermediate file

This would be nice for github actions too. Alternatively offer a way to specify the system interpreter when doing `uv sync`

---

_Comment by @zanieb on 2024-08-27 17:29_

@shaunhegarty can you share more about why `uv run` is not suitable for use in GitHub Actions in your use-case?

---

_Comment by @shaunhegarty on 2024-08-27 18:04_

> @shaunhegarty can you share more about why `uv run` is not suitable for use in GitHub Actions in your use-case?

@zanieb Knowledge deficit on my part. `uv run` works for all the actions so far. Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 23:11_

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---
