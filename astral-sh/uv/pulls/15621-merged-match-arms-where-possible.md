```yaml
number: 15621
title: merged match arms where possible
type: pull_request
state: open
author: adamnemecek
labels:
  - internal
assignees: []
base: main
head: uv10
created_at: 2025-09-01T15:26:51Z
updated_at: 2025-09-03T16:56:58Z
url: https://github.com/astral-sh/uv/pull/15621
synced_at: 2026-01-10T06:44:33Z
```

# merged match arms where possible

---

_Pull request opened by @adamnemecek on 2025-09-01 15:26_

_No description provided._

---

_Comment by @konstin on 2025-09-01 15:34_

I personally like this style, but if we want to use it, we should activate the clippy rule for it too ([source](https://github.com/astral-sh/uv/blob/22f80ca00d3f5af7d087154e998c886bbea8cee1/Cargo.toml#L234)) CC @charliermarsh 

---

_Comment by @zanieb on 2025-09-02 13:12_

Also noted at https://github.com/astral-sh/uv/pull/15295#issuecomment-3192958323

---

_Comment by @adamnemecek on 2025-09-02 17:40_

Can I add do in another PR? Fixing all of these will take some time as for some reason clippy cannot autofix this.

---

_Comment by @konstin on 2025-09-03 13:26_

What was trying to get to, is that we should decide whether we want to toggle the clippy rule before we merge this PR. Once we have that decision, we can do the actual changes in a sequence of PRs.

---

_Label `internal` added by @konstin on 2025-09-03 13:26_

---

_Comment by @adamnemecek on 2025-09-03 16:56_

Ok, I guess that decision is up to you.

---
