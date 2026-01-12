```yaml
number: 2969
title: Unify packse find links urls
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/unify-packse-find-links
created_at: 2024-04-10T15:14:27Z
updated_at: 2024-04-11T08:35:24Z
url: https://github.com/astral-sh/uv/pull/2969
synced_at: 2026-01-12T16:05:20Z
```

# Unify packse find links urls

---

_@konstin_

The sync scenarios script is broken, so i did the updates manually

```
$ ./scripts/sync_scenarios.sh
Setting up a temporary environment...
Using Python 3.12.1 interpreter at: /home/konsti/projects/uv/.venv/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
  × No solution found when resolving dependencies:
  ╰─▶ Because docutils==0.21.post1 is unusable because the package metadata was inconsistent and you require docutils==0.21.post1, we can conclude that the requirements are unsatisfiable.

      hint: Metadata for docutils==0.21.post1 was inconsistent:
        Package metadata version `0.21` does not match given version `0.21.post1`
```

---

_Label `internal` added by @konstin on 2024-04-10 15:14_

---

_Review requested from @zanieb by @konstin on 2024-04-10 15:14_

---

_@zanieb reviewed on 2024-04-10 15:20_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:27 on 2024-04-10 15:20_

Should we call it like `BUILD_VENDOR_LINKS_URL` to make it clearer whats in here?

---

_Comment by @charliermarsh on 2024-04-10 15:23_

> The sync scenarios script is broken

I can't reproduce this, any idea what's going on there?

---

_Comment by @konstin on 2024-04-10 15:27_

The docutils source dist is inconsistent about its version number: https://inspector.pypi.io/project/docutils/0.21/ . Haven't checked yet where that breaks uv

---

_Comment by @charliermarsh on 2024-04-10 15:28_

Note that I changed / fixed this behavior last night: https://github.com/astral-sh/uv/issues/2953. But why is your system using the source distribution?

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:27 on 2024-04-10 15:30_

It'd be nice to document why it's useful too. I added this so we could use `--index-url` instead of `--extra-index-url` to prevent dependency confusion attacks against our test suite, but without vendoring build dependencies that will generally fail.

---

_@zanieb reviewed on 2024-04-10 15:30_

---

_Comment by @zanieb on 2024-04-10 16:09_

I tested with the sync script in https://github.com/astral-sh/uv/pull/2970

---

_Merged by @konstin on 2024-04-11 08:35_

---

_Closed by @konstin on 2024-04-11 08:35_

---

_Branch deleted on 2024-04-11 08:35_

---
