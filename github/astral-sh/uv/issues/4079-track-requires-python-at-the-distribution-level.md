---
number: 4079
title: "Track `Requires-Python` at the distribution level in lockfile"
type: issue
state: open
author: charliermarsh
labels: []
assignees: []
created_at: 2024-06-05T22:39:51Z
updated_at: 2024-08-20T18:23:37Z
url: https://github.com/astral-sh/uv/issues/4079
synced_at: 2026-01-07T13:12:17-06:00
---

# Track `Requires-Python` at the distribution level in lockfile

---

_Issue opened by @charliermarsh on 2024-06-05 22:39_

A wheel and a source distribution for the same package-version could have different `Requires-Python` values. We respect this when resolving, but we lose `Requires-Python` when we serialize the lockfile and then reify the resolution.

(Or, at least, this is "allowed" in the API. We may want to assume a fixed requirement for a package-version just as we assume fixed metadata for a package-version? I'm not sure if this is common. If it's common, we should track and respect it in the lockfile.)


---

_Label `preview` added by @charliermarsh on 2024-06-05 22:39_

---

_Comment by @konstin on 2024-07-09 10:45_

I think this is a non-issue for us, since we assume core metadata to be the same for all distributions of a release and `Requires-Python` is part of the core metadata

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---
