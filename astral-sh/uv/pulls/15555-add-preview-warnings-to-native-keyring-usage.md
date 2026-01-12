```yaml
number: 15555
title: "Add preview warnings to `native-keyring` usage"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/uv-auth-iii
created_at: 2025-08-27T17:30:49Z
updated_at: 2025-08-28T15:31:11Z
url: https://github.com/astral-sh/uv/pull/15555
synced_at: 2026-01-12T16:11:49Z
```

# Add preview warnings to `native-keyring` usage

---

_@zanieb_

The refactor here was all done by Claude Code.

---

_Comment by @zanieb on 2025-08-27 21:38_

I believe the CI failure here will be resolved by #15564 

---

_@charliermarsh approved on 2025-08-28 01:08_

---

_Comment by @charliermarsh on 2025-08-28 01:08_

Not specific to this PR, but I wonder if we should use some kind of singleton so that we don't have to wire `preview` everywhere. E.g., a global mutable `preview` thing that we initialize at the start of the program...

---

_Comment by @zanieb on 2025-08-28 13:17_

> Not specific to this PR, but I wonder if we should use some kind of singleton so that we don't have to wire `preview` everywhere. E.g., a global mutable `preview` thing that we initialize at the start of the program...

Yeah, we've talked about it. Then you lose some visibility into which code paths have preview behaviors, but... could definitely be worth it.

---

_Marked ready for review by @zanieb on 2025-08-28 13:18_

---

_Comment by @konstin on 2025-08-28 14:50_

If we merge https://github.com/astral-sh/uv/pull/15548 first, we can remove most of the non-test changes, the preview feature gets passed in the base client builder once before the cli command match. For the test usages, we can set the default preview mode in `BaseClientBuilder::default()`.

---

_Merged by @zanieb on 2025-08-28 15:30_

---

_Closed by @zanieb on 2025-08-28 15:30_

---

_Branch deleted on 2025-08-28 15:30_

---

_Comment by @zanieb on 2025-08-28 15:31_

(Just going into a feature branch, can drop a bunch of it as you said if your PR lands)

---
