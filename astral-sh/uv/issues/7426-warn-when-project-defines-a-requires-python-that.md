```yaml
number: 7426
title: "Warn when project defines a `requires-python` that pins to a `.0` patch version"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - projects
assignees: []
created_at: 2024-09-16T13:38:58Z
updated_at: 2025-06-27T20:48:43Z
url: https://github.com/astral-sh/uv/issues/7426
synced_at: 2026-01-12T15:59:13Z
```

# Warn when project defines a `requires-python` that pins to a `.0` patch version

---

_@zanieb_

As described in https://github.com/astral-sh/uv/issues/7352#issuecomment-2352928006

If the user defines a Python requirement of `==3.12` this means `==3.12.0` which I don't think anyone really wants. We should suggest using `==3.12.*` instead?

---

_Label `projects` added by @zanieb on 2024-09-16 13:39_

---

_Comment by @edmorley on 2024-09-25 19:10_

Similarly, it may be worth warning about `~=X.Y` too (since it's confusingly not the same as `~=X.Y.0`), xref #7682.

---

_Label `good first issue` added by @zanieb on 2024-09-25 19:36_

---

_Comment by @ianpaul10 on 2024-10-03 16:55_

Looking to get started on uv, I can take a look at this if no one has had a chance to tackle it yet.

---

_Comment by @zanieb on 2024-10-03 16:58_

Feel free!

---

_Comment by @ryan-ph on 2025-01-25 06:45_

It seems the linked PR hasn't been updated in a while. Is this something that we would like to warn on? If so, happy to take this.

---

_Comment by @zanieb on 2025-05-27 16:17_

I've closed https://github.com/astral-sh/uv/pull/8284 — feel free to pick that work up!

---

_Comment by @aaron-ang on 2025-06-12 17:16_

Hi @ryan-ph, are you working on this issue?

---

_Comment by @ryan-ph on 2025-06-12 23:58_

@aaron-ang: I am not. Thanks for the PR!

---

_Comment by @aaron-ang on 2025-06-27 19:28_

hi @zanieb could you take a look at #14008 when you have time? Hopefully we can close this issue :)

---

_Closed by @zanieb on 2025-06-27 20:48_

---
