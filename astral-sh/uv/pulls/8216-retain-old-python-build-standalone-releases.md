```yaml
number: 8216
title: Retain old python-build-standalone releases
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-old-dists
created_at: 2024-10-15T13:28:35Z
updated_at: 2024-10-15T16:08:48Z
url: https://github.com/astral-sh/uv/pull/8216
synced_at: 2026-01-10T12:54:04Z
```

# Retain old python-build-standalone releases

---

_Pull request opened by @zanieb on 2024-10-15 13:28_

Closes https://github.com/astral-sh/uv/issues/8213

I didn't mean to remove these when updating the regular expression. Arguably, they shouldn't be used anymore, but we should make that choice with intention.

---

_Label `bug` added by @zanieb on 2024-10-15 13:28_

---

_Comment by @zanieb on 2024-10-15 13:40_

Wow it's actually horrible to try to construct the regex to match all these. We need to futz with a second lenient regex or just drop support.

---

_@charliermarsh approved on 2024-10-15 14:38_

---

_Comment by @zanieb on 2024-10-15 14:39_

Struggling to validate that the changes here are "correct"

---

_Comment by @notatallshaw-gts on 2024-10-15 15:06_

> Arguably, they shouldn't be used anymore, but we should make that choice with intention.

Two things from my point of view:

One, there is a strong use case for pinning against a patch version of Python, I have seen plenty of times where patch versions of Python breaks code, so I want to be able to control when a patch version is updated and test against it carefully.

Two, I would not expect a tool that installs my Python versions to *ever* make certain versions unavailable without announcing it as a big breaking change. This will be particularly important for large organizations, but if I have an internal app running against Python 3.7.9, and everything is pinned, I would not expect that to suddenly not work even if that app is still running against that version of Python in 10 years.

---

_Comment by @zanieb on 2024-10-15 15:23_

>  I would not expect a tool that installs my Python versions to ever make certain versions unavailable without announcing it as a big breaking change. 

Definitely agree — this was an unintentionally removal due to the peculiarities of these old artifacts.

Of course, this still works fine if you're using an old version of uv (and I think if you're going to be pinning and using things 10 years out you'll want to pin that!)

---

_Merged by @zanieb on 2024-10-15 16:08_

---

_Closed by @zanieb on 2024-10-15 16:08_

---

_Branch deleted on 2024-10-15 16:08_

---
