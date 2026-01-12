```yaml
number: 12931
title: "Add tests for `uv python list`"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
base: main
head: list-only-downloads-all-arches
created_at: 2025-04-17T04:03:08Z
updated_at: 2025-11-01T15:51:58Z
url: https://github.com/astral-sh/uv/pull/12931
synced_at: 2026-01-12T16:10:27Z
```

# Add tests for `uv python list`

---

_@j178_

# Summary

I added a subset of the Python downloads metadata json file for tests.

~This PR also fixes a issue where using `--only-downloads` with `--all-arches`, `--all-arches` is not honored.~


---

_Closed by @j178 on 2025-04-17 05:22_

---

_Reopened by @j178 on 2025-04-17 05:22_

---

_Review requested from @zanieb by @konstin on 2025-04-17 09:41_

---

_Assigned to @zanieb by @zanieb on 2025-04-17 17:01_

---

_@alexprengere reviewed on 2025-04-17 17:48_

---

_Review comment by @alexprengere on `crates/uv/src/commands/python/list.rs`:82 on 2025-04-17 17:48_

This needs to be rebased and updated using fill_platform, now that #12915 has been merged to main.

---

_@zanieb reviewed on 2025-04-17 21:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/python-downloads-metadata.json`:1 on 2025-04-17 21:17_

Should this be here?

---

_@zanieb reviewed on 2025-04-17 21:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:82 on 2025-04-17 21:17_

Is this a bug fix?

---

_@zanieb reviewed on 2025-04-17 21:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:82 on 2025-04-17 21:17_

Generally, it'd be better to do this separately so it can be listed in the changelog.

---

_@zanieb reviewed on 2025-04-17 21:20_

---

_Review comment by @zanieb on `crates/uv/tests/it/python-downloads-metadata.json`:1 on 2025-04-17 21:20_

Ah I see it's being used for the test case. Interesting 🤔 I'm worried about maintaining these. Do we need a subset? If so, should it be much smaller? Can you say more about the choice?

---

_@j178 reviewed on 2025-04-17 21:36_

---

_Review comment by @j178 on `crates/uv/src/commands/python/list.rs`:82 on 2025-04-17 21:36_

Extracted it to #12953 

---

_@j178 reviewed on 2025-04-17 21:41_

---

_Review comment by @j178 on `crates/uv/tests/it/python-downloads-metadata.json`:1 on 2025-04-17 21:41_

I think we need a fixed subset to keep things stable; otherwise, the tests fall apart every time we sync the latest Python versions.

So, I’m making sure the subset includes a prerelease, a freethreaded variant, different patch versions of the same minor version, and different minor versions. This way, we’ve got a pretty thorough test case.

---

_@zanieb reviewed on 2025-04-21 19:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/python-downloads-metadata.json`:1 on 2025-04-21 19:45_

Hm.

I'm not sure if I mind if the snapshots need updating when the available versions change.

My concern is that the snapshot doesn't capture the reality for most users, which makes it less valuable and more likely for us to accidentally ship regressions.

---

_Comment by @zanieb on 2025-04-21 19:46_

Note this overlaps with https://github.com/astral-sh/uv/pull/12381 which I forgot to follow-up on after https://github.com/astral-sh/uv/pull/12381#issuecomment-2744326955.

---

_Closed by @j178 on 2025-11-01 15:51_

---
