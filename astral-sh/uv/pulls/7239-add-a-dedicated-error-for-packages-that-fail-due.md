```yaml
number: 7239
title: "Add a dedicated error for packages that fail due to `distutils` deprecation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/versiomn
created_at: 2024-09-10T01:37:30Z
updated_at: 2024-09-12T02:41:43Z
url: https://github.com/astral-sh/uv/pull/7239
synced_at: 2026-01-10T12:53:43Z
```

# Add a dedicated error for packages that fail due to `distutils` deprecation

---

_Pull request opened by @charliermarsh on 2024-09-10 01:37_

## Summary

Closes https://github.com/astral-sh/uv/issues/7183.

## Test Plan

![Screenshot 2024-09-09 at 9 33 45 PM](https://github.com/user-attachments/assets/ffe896cc-d5dd-4ec8-96c9-c37a964489e3)


---

_Renamed from "Add a dedicated message for packages that fail due to `distutils` deprecation" to "Add a dedicated error for packages that fail due to `distutils` deprecation" by @charliermarsh on 2024-09-10 01:37_

---

_Label `error messages` added by @charliermarsh on 2024-09-10 01:37_

---

_Review requested from @konstin by @charliermarsh on 2024-09-10 01:37_

---

_@zanieb approved on 2024-09-10 02:34_

Nice

---

_Merged by @charliermarsh on 2024-09-10 02:51_

---

_Closed by @charliermarsh on 2024-09-10 02:51_

---

_Branch deleted on 2024-09-10 02:51_

---

_Review comment by @edmorley on `crates/uv-build/src/error.rs`:128 on 2024-09-10 16:12_

I love this type of specific error handling btw (I really miss the Cargo style great error messages in Python in general). 

For context, over half of Python app build failures on our platform fail during the `pip install` step (not surprising given it's the biggest point of user-provided input), so improving UX there is something I'm very interested in. 

We've tried a small amount of log parsing based suggestions in the past, but (a) hadn't had a chance to put more effort into them, (b) the package manager really has more context than something wrapping it to try and make more intelligent suggestions - so having uv handle these instead of the buildpack would be ideal.

I presume you would be interested to know more about any real world failure modes that are confusing for users, for cases where uv doesn't already handle them well? (For now all such examples will be based on pip, but I'm sure most will translate.)


---

_@edmorley reviewed on 2024-09-10 16:12_

---

_Review comment by @charliermarsh on `crates/uv-build/src/error.rs`:128 on 2024-09-10 18:51_

Yes, absolutely. Any data or info you can share on common errors would be much appreciated.

---

_@charliermarsh reviewed on 2024-09-10 18:51_

---
