```yaml
number: 6791
title: Update the Git fetch API to be async
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/git-fetch-async
created_at: 2024-08-29T04:28:24Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/6791
synced_at: 2026-01-12T16:07:32Z
```

# Update the Git fetch API to be async

---

_@zanieb_

Putting this up as a follow-up to https://github.com/astral-sh/uv/pull/6790

Drops the awkward nested tokio runtime in favor of making parts of the Git fetch API async. More work will be required to wrap some of the Git CLI calls in `spawn_blocking` to avoid blocking the runtime during slow subprocess invocations.

---

_@zanieb reviewed on 2024-08-29 04:29_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:723 on 2024-08-29 04:29_

We can remove this, which is nice.

---

_Comment by @charliermarsh on 2024-08-29 16:57_

I'm still in favor of this personally!

---

_Comment by @zanieb on 2024-08-29 17:01_

Yeah we just need to make sure we're doing the right thing in all the subprocess calls — it will be a second before I prioritize this.

---
