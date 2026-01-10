```yaml
number: 15940
title: Sync latest Python releases
type: pull_request
state: merged
author: github-actions
labels:
  - no-build
  - no-test
assignees: []
merged: true
base: main
head: sync-python-releases
created_at: 2025-09-18T23:42:15Z
updated_at: 2025-09-19T18:13:39Z
url: https://github.com/astral-sh/uv/pull/15940
synced_at: 2026-01-10T06:36:15Z
```

# Sync latest Python releases

---

_Pull request opened by @github-actions on 2025-09-18 23:42_

Automated update for Python releases.

---

_Closed by @zanieb on 2025-09-18 23:50_

---

_Reopened by @zanieb on 2025-09-18 23:50_

---

_Comment by @geofft on 2025-09-19 00:14_

@zanieb as we get closer to 3.14 final, what do you want to do about the prerelease tests? I guess 3.15a0 will show up a week afterwards and we can just skip this test for that week?

---

_Comment by @zanieb on 2025-09-19 01:20_

Yeah or we can add some sort of exclude newer for our Python distributions.

---

_Comment by @konstin on 2025-09-19 14:11_

CI doesn't seem to like this one right now:

> error: No download found for request: cpython-3.14rc3-linux-x86_64-gnu

---

_Comment by @geofft on 2025-09-19 14:43_

Oops, I shouldn't have updated .python-versions yet because that goes into an existing version of uv :)

Although this might still not be right, let's see

---

_Comment by @geofft on 2025-09-19 15:37_

Can one of y'all look at the comments I added and make sure they are accurate?

---

_@zanieb reviewed on 2025-09-19 17:09_

---

_Review comment by @zanieb on `.python-versions`:9 on 2025-09-19 17:09_

Well, they're installed through the `uv` that comes from that action

https://github.com/astral-sh/uv/blob/00aa2ab67290f45c3797c1359d027cc38d137d7e/.github/workflows/ci.yml#L231-L233

but not that action

---

_@zanieb reviewed on 2025-09-19 17:09_

---

_Review comment by @zanieb on `.python-versions`:9 on 2025-09-19 17:09_

Otherwise, this is correct, though I think it's a niche problem to encounter.

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:912 on 2025-09-19 17:10_

I don't think we should document this here, though I understand it bit you. That's how `new_with_versions` works in all cases.

---

_@zanieb reviewed on 2025-09-19 17:10_

---

_Label `no-build` added by @geofft on 2025-09-19 18:12_

---

_Label `no-test` added by @geofft on 2025-09-19 18:12_

---

_Merged by @geofft on 2025-09-19 18:13_

---

_Closed by @geofft on 2025-09-19 18:13_

---

_Branch deleted on 2025-09-19 18:13_

---

_Comment by @geofft on 2025-09-19 18:13_

Rephrased and dropped the second comment, thanks! I'm disabling CI since it just affects comments and merging and keeping an eye on HEAD just in case I screwed something up.

---
