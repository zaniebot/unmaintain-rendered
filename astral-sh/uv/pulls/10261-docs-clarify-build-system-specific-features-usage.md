```yaml
number: 10261
title: "docs: Clarify build system specific features usage."
type: pull_request
state: merged
author: FishAlchemist
labels:
  - documentation
assignees: []
merged: true
base: main
head: supplementary_build-system
created_at: 2025-01-01T10:38:41Z
updated_at: 2025-01-09T18:08:05Z
url: https://github.com/astral-sh/uv/pull/10261
synced_at: 2026-01-10T11:44:39Z
```

# docs: Clarify build system specific features usage.

---

_Pull request opened by @FishAlchemist on 2025-01-01 10:38_

## Summary
Since there are occasional inquiries about how to configure UV for build-system specific features, I want to raise awareness that users should refer to the documentation of the build system they are using for relevant settings.
## Test Plan
Run docs service in local.
https://github.com/astral-sh/uv/pull/10261/commits/9821d58d35dce6bbafe4bdadd9031afba0f787a5
![image](https://github.com/user-attachments/assets/3c07ac15-a562-40e2-9289-204c0975261f)



---

_Comment by @FishAlchemist on 2025-01-01 10:38_

However, if you have a better approach, please feel free to ``add a suggestion`` or create a separate pull request. If you think this is unnecessary, you can close this pull request directly.

If we require certain files to be included, we'll need to read the setuptools documentation for configuration options.
* https://github.com/astral-sh/uv/issues/10254

---

_Review requested from @zanieb by @charliermarsh on 2025-01-01 19:53_

---

_Label `documentation` added by @charliermarsh on 2025-01-01 19:53_

---

_Assigned to @zanieb by @zanieb on 2025-01-08 18:53_

---

_Comment by @zanieb on 2025-01-08 21:51_

Thanks!

I pushed https://github.com/astral-sh/uv/pull/10261/commits/859b6b0e50aaa5b3cf104a370a65840229c890fe to give this some dedicated space

Is there more we should add here? I'm sure the bullets are not exhaustive as is. cc @konstin  

---

_Comment by @konstin on 2025-01-09 08:12_

There's also:
* dynamic metadata
* native code compilation
* partially: shared library vendoring/glibc compatibility/auditwheel (maturin integrates it, other backends expect the user to run auditwheel).

---

_@zanieb approved on 2025-01-09 14:39_

---

_Comment by @FishAlchemist on 2025-01-09 15:21_

I just synced with main (git rebase main), but I don't know how to turn off signing commits.
By the way, have we considered running the ``uvx ruff version`` in CI? Because I really can't reproduce the error.

---

_Comment by @FishAlchemist on 2025-01-09 15:25_

The issue seems to be related to ruff 0.9.0's handling of f-string expressions.

---

_Comment by @konstin on 2025-01-09 15:27_

The error is not your fault, it's unrelated to your PR. The ruff 0.9 upgrade broke uv CI because uv CI doesn't pin ruff and it's getting fixed in https://github.com/astral-sh/uv/pull/10433. Once that is merged we can rebase this PR on main and merge.

---

_Merged by @zanieb on 2025-01-09 17:41_

---

_Closed by @zanieb on 2025-01-09 17:41_

---

_Branch deleted on 2025-01-09 18:08_

---
