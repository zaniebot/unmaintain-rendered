```yaml
number: 3995
title: Bias towards local directories for bare editable requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pref
created_at: 2024-06-03T19:23:30Z
updated_at: 2024-06-04T19:37:06Z
url: https://github.com/astral-sh/uv/pull/3995
synced_at: 2026-01-10T13:54:02Z
```

# Bias towards local directories for bare editable requirements

---

_Pull request opened by @charliermarsh on 2024-06-03 19:23_

## Summary

Given `install -e dagster`, we need to assume that the user meant `install -e ./dagster`, even though `install dagster` should _not_ be treated as `install ./dagster`. I suspect pip will change this in the future (since `pip install dagster` does _not_ meant `pip install ./dagster`) but for now it's what users expect.

Closes https://github.com/astral-sh/uv/issues/3994.


---

_Label `bug` added by @charliermarsh on 2024-06-03 19:23_

---

_Marked ready for review by @charliermarsh on 2024-06-03 19:23_

---

_Comment by @charliermarsh on 2024-06-03 19:23_

Needs tests.

---

_Comment by @codspeed-hq[bot] on 2024-06-03 19:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/pref)

### Merging #3995 will **not alter performance**

<sub>Comparing <code>charlie/pref</code> (787920a) with <code>main</code> (420333a)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @konstin on 2024-06-04 09:00_

Is there a pip issue to subscribe to?

---

_@konstin approved on 2024-06-04 09:01_

---

_Comment by @charliermarsh on 2024-06-04 13:25_

I only see https://github.com/pypa/pip/issues/1289#issuecomment-1510735040 where they mention that lack of PEP 508 support for editables is an implementation issue, not a design issue. But if they change editables to support PEP 508, I think they'll have the same behavior as us (prior to this PR), unless they also special-case it?

---

_@zanieb approved on 2024-06-04 13:41_

---

_Comment by @charliermarsh on 2024-06-04 13:54_

I asked in pip here: https://github.com/pypa/pip/issues/12745

---

_Merged by @charliermarsh on 2024-06-04 19:37_

---

_Closed by @charliermarsh on 2024-06-04 19:37_

---

_Branch deleted on 2024-06-04 19:37_

---
