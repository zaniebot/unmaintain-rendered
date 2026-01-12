```yaml
number: 5411
title: Use env variables in Github Actions docs
type: pull_request
state: merged
author: blueraft
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: doc-env-suggestion
created_at: 2024-07-24T15:15:20Z
updated_at: 2024-07-24T19:38:26Z
url: https://github.com/astral-sh/uv/pull/5411
synced_at: 2026-01-12T16:06:48Z
```

# Use env variables in Github Actions docs

---

_@blueraft_

## Summary

Use [Github Actions](https://docs.github.com/en/actions/learn-github-actions/variables#defining-environment-variables-for-a-single-workflow) `env` to define environment variables.

After:
<img width="751" alt="Screenshot 2024-07-24 at 17 24 23" src="https://github.com/user-attachments/assets/9c7861f0-e95d-4778-96c0-261df539a0bd">



---

_@blueraft reviewed on 2024-07-24 15:16_

---

_Review comment by @blueraft on `docs/guides/integration/github.md`:169 on 2024-07-24 15:16_

I think this shell command might not work on Windows, should we mention it's unix specific?

---

_@zanieb reviewed on 2024-07-24 15:18_

---

_Review comment by @zanieb on `docs/guides/integration/github.md`:169 on 2024-07-24 15:18_

Hm interesting yeah seems like it might need `>> $env:GITHUB_ENV` on Windows. Let's just drop it — no reason to do it this way really.

---

_@blueraft reviewed on 2024-07-24 15:23_

---

_Review comment by @blueraft on `docs/guides/integration/github.md`:169 on 2024-07-24 15:23_

Done

---

_Assigned to @zanieb by @zanieb on 2024-07-24 15:25_

---

_Label `documentation` added by @zanieb on 2024-07-24 15:25_

---

_Label `preview` added by @zanieb on 2024-07-24 15:25_

---

_@zanieb approved on 2024-07-24 19:37_

Thanks!

---

_Merged by @zanieb on 2024-07-24 19:37_

---

_Closed by @zanieb on 2024-07-24 19:37_

---

_Branch deleted on 2024-07-24 19:38_

---
