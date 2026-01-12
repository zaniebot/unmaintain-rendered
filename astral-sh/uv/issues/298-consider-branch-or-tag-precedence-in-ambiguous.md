```yaml
number: 298
title: Consider branch or tag precedence in ambiguous git references
type: issue
state: open
author: zanieb
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2023-11-02T19:12:01Z
updated_at: 2025-12-02T09:43:23Z
url: https://github.com/astral-sh/uv/issues/298
synced_at: 2026-01-12T15:58:22Z
```

# Consider branch or tag precedence in ambiguous git references

---

_@zanieb_

We currently will use a branch name over a tag name if they both exist.

Similarly, `git checkout` prefers switching to a branch:

```
❯ git tag v0.1.0
❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ gcb v0.1.0
Switched to a new branch 'v0.1.0'
❯ gcm
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
❯ gco v0.1.0
warning: refname 'v0.1.0' is ambiguous.
Switched to branch 'v0.1.0'
```

but for _installing_ packages I would much rather match a tag than a branch if there's a collision.

_Originally posted by @zanieb in https://github.com/astral-sh/puffin/pull/297#discussion_r1380659592_
            

---

_Comment by @zanieb on 2023-11-02 19:13_

It'd probably be good to warn as part of #287 if there's an ambiguous ref.

---

_Label `enhancement` added by @charliermarsh on 2023-11-02 20:11_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-02 20:11_

---

_Removed from milestone `Future` by @konstin on 2024-07-09 12:37_

---

_Comment by @Aditya-PS-05 on 2025-02-12 12:53_

Is this issue, currently open? If yes, I would like to solve it.

---

_Comment by @zanieb on 2025-02-12 13:45_

I'm not sure we're aligned on if this should be done, let me check in with the team first

---

_Label `needs-decision` added by @zanieb on 2025-12-02 09:43_

---
