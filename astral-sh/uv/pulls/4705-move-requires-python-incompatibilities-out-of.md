```yaml
number: 4705
title: "Move `Requires-Python` incompatibilities out of version map"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/python-enforcement
created_at: 2024-07-01T20:22:33Z
updated_at: 2024-07-02T12:15:42Z
url: https://github.com/astral-sh/uv/pull/4705
synced_at: 2026-01-10T13:48:28Z
```

# Move `Requires-Python` incompatibilities out of version map

---

_Pull request opened by @charliermarsh on 2024-07-01 20:22_

## Summary

This is required to solve https://github.com/astral-sh/uv/issues/4669, because the `Requires-Python` version can now vary across a resolution. For example, within certain forks, we might have a more narrow range, which would allow us to use distributions that would not be allowed for the global resolution.

This should be fine because `requires-python` is part of the package metadata, so it should be consistent between files within a package version. As such, there shouldn't be any risk that we incorrectly prioritize distributions by omitting this information.

(To be more specific, the risk is something like: we prioritize some wheel over a source distribution within a package-version, so we don't track the source distribution at all. Then, later, when we choose a candidate, we see that the wheel doesn't meet the `Requires-Python` requirement, even though the source distribution _would've_ met it. If files within a distribution could have varied support, this would be a real risk.)


---

_Review requested from @zanieb by @charliermarsh on 2024-07-01 20:22_

---

_Review requested from @konstin by @charliermarsh on 2024-07-01 20:22_

---

_Label `internal` added by @charliermarsh on 2024-07-01 20:22_

---

_@zanieb approved on 2024-07-01 21:07_

Interesting 👍 

---

_Comment by @zanieb on 2024-07-01 21:09_

Originally moved here in https://github.com/astral-sh/uv/pull/1298

---

_Comment by @charliermarsh on 2024-07-01 21:09_

I think having it in the version map is actually nice, but it's no longer possible unfortunately.

---

_@konstin approved on 2024-07-02 10:34_

---

_Merged by @charliermarsh on 2024-07-02 12:15_

---

_Closed by @charliermarsh on 2024-07-02 12:15_

---

_Branch deleted on 2024-07-02 12:15_

---
