```yaml
number: 11328
title: Set 777 permissions on locked files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/perms
created_at: 2025-02-07T20:13:50Z
updated_at: 2025-02-07T21:36:11Z
url: https://github.com/astral-sh/uv/pull/11328
synced_at: 2026-01-12T16:09:47Z
```

# Set 777 permissions on locked files

---

_@charliermarsh_

## Summary

These are used for coordination across processes. If you run uv under, e.g., the root user, then under a different user, I don't think we should prevent you from acquiring the lock.

Closes https://github.com/astral-sh/uv/issues/11324.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-02-07 20:13_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-07 20:13_

---

_Label `bug` added by @charliermarsh on 2025-02-07 20:13_

---

_Comment by @charliermarsh on 2025-02-07 20:17_

I think I'm separately going to make "failed to acquire the lock" a non-fatal error.

---

_@BurntSushi approved on 2025-02-07 20:21_

I think this is probably fine. The only downside I can think of is that, by making them `0777`, some other user could futz with your lock files potentially. But I think the benefits here probably outweigh that downside.

---

_Comment by @charliermarsh on 2025-02-07 20:23_

Is there any sort of vague concern around this interacting poorly with a custom umask?

---

_Comment by @charliermarsh on 2025-02-07 20:27_

Huh, ok, this actually didn't fix https://github.com/astral-sh/uv/issues/11324?

---

_Comment by @BurntSushi on 2025-02-07 20:27_

I don't have one off the top of my head. I'm only like 75% confident in that though.

---

_Comment by @charliermarsh on 2025-02-07 20:29_

It looks like the umask is 0022 so I have to set this _after_ creation? Hmm...

---

_Review request for @zanieb removed by @charliermarsh on 2025-02-07 21:01_

---

_Review requested from @Gankra by @charliermarsh on 2025-02-07 21:01_

---

_@zanieb approved on 2025-02-07 21:13_

---

_Merged by @charliermarsh on 2025-02-07 21:36_

---

_Closed by @charliermarsh on 2025-02-07 21:36_

---

_Branch deleted on 2025-02-07 21:36_

---
