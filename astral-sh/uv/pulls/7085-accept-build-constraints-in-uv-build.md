```yaml
number: 7085
title: "Accept `--build-constraints` in `uv build`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/build-constraints
created_at: 2024-09-05T15:25:40Z
updated_at: 2024-09-05T18:46:37Z
url: https://github.com/astral-sh/uv/pull/7085
synced_at: 2026-01-12T16:07:41Z
```

# Accept `--build-constraints` in `uv build`

---

_@charliermarsh_

## Summary

Closes #7082.

Closes #7065.


---

_Label `enhancement` added by @charliermarsh on 2024-09-05 15:25_

---

_Label `cli` added by @charliermarsh on 2024-09-05 15:25_

---

_Comment by @alex on 2024-09-05 15:35_

Thanks for jumping on this so quickly! I assume that hashes in the constraint file will automatically be enforced?

---

_Comment by @charliermarsh on 2024-09-05 15:36_

Actually not certain, let me look into it.

---

_Comment by @zanieb on 2024-09-05 15:42_

Probably worth mention in https://docs.astral.sh/uv/concepts/projects/#building-projects

---

_@zanieb approved on 2024-09-05 15:42_

---

_Comment by @charliermarsh on 2024-09-05 15:44_

It looks like we don't currently grab hashes from constraints files. I will fix that first, separately.

---

_Comment by @alex on 2024-09-05 15:53_

Thanks much!

---

_@charliermarsh reviewed on 2024-09-05 18:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build.rs`:246 on 2024-09-05 18:17_

So... this will validate any hashes that are provided in the constraints file. But it doesn't _require_ hashes. Should we add a separate flag to require them?

---

_@alex reviewed on 2024-09-05 18:18_

---

_Review comment by @alex on `crates/uv/src/commands/build.rs`:246 on 2024-09-05 18:18_

I'd love that.

❤️ ❤️ ❤️ 

---

_Merged by @charliermarsh on 2024-09-05 18:46_

---

_Closed by @charliermarsh on 2024-09-05 18:46_

---

_Branch deleted on 2024-09-05 18:46_

---
