```yaml
number: 14913
title: Add Python versions to markers implied from wheels
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/python-implied-markers
created_at: 2025-07-26T04:06:37Z
updated_at: 2025-08-05T19:52:34Z
url: https://github.com/astral-sh/uv/pull/14913
synced_at: 2026-01-10T06:44:33Z
```

# Add Python versions to markers implied from wheels

---

_Pull request opened by @zanieb on 2025-07-26 04:06_

Looking into  https://github.com/astral-sh/uv/issues/14836

This does resolve the issue, if the user adds `python_version == '3.8.*'` to the `required-environments`.


---

_Comment by @zanieb on 2025-07-29 23:35_

In contrast to platform markers, I think we might _always_ want to enforce that the Python version markers are satisfied by the implied markers, even if they're not defined as a required-environment. However, I'm curious if we should land this pull request _first_ since it's an incremental improvement and we can get a sense of whether or not this has unintended effects? Then we can land doing this by default as a subsequent change?

---

_Comment by @charliermarsh on 2025-07-30 01:16_

Yeah, I think we _should_ always enforce this, unlike platform markers.

I'm okay shipping this first if you'd prefer.

---

_Comment by @charliermarsh on 2025-07-30 01:17_

The only downside here is that the lockfile markers get unnecessarily more-complex in some cases, as you can see from the diffs.

---

_Comment by @zanieb on 2025-07-30 12:58_

Agree it's a bummer the markers are more complex. I guess some of that noise would be cut out if we treated `platform_python_implementation` differently. I think it's sort of fine though.


---

_Marked ready for review by @zanieb on 2025-07-30 23:03_

---

_Comment by @zanieb on 2025-07-30 23:04_

> Yeah, I think we should always enforce this, unlike platform markers.

I'll try to look into that in the next week or so.

---

_Label `bug` added by @zanieb on 2025-08-04 23:33_

---

_Merged by @zanieb on 2025-08-05 19:52_

---

_Closed by @zanieb on 2025-08-05 19:52_

---

_Branch deleted on 2025-08-05 19:52_

---
