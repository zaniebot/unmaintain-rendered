```yaml
number: 6119
title: "Return a structured result from `Lock::satisfies`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/satisfies
created_at: 2024-08-15T16:13:32Z
updated_at: 2024-08-15T17:19:41Z
url: https://github.com/astral-sh/uv/pull/6119
synced_at: 2026-01-12T16:07:13Z
```

# Return a structured result from `Lock::satisfies`

---

_@charliermarsh_

## Summary

Gives the caller control over how messages are reported back to the user. Also merges the index-location validation into the lock, since we're already iterating over the packages.


---

_Label `internal` added by @charliermarsh on 2024-08-15 16:13_

---

_Label `preview` added by @charliermarsh on 2024-08-15 16:13_

---

_Label `lock` added by @charliermarsh on 2024-08-15 16:13_

---

_@charliermarsh reviewed on 2024-08-15 16:13_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:8206 on 2024-08-15 16:13_

I made this all debug. Not sure...

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:8206 on 2024-08-15 16:21_

I think I might be inclined to show it. Or at least, I feel like we should show something indicating that we did (or tried to) do something to update the lock file. I think Cargo does this.

---

_@BurntSushi approved on 2024-08-15 16:21_

Makes sense!

---

_@charliermarsh reviewed on 2024-08-15 16:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:8206 on 2024-08-15 16:52_

@zanieb -- Do you have an opinion on whether any of these "We can't reuse the lockfile because..." messages should be printed to stderr by default, or as `debug!`?

---

_@zanieb reviewed on 2024-08-15 17:16_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:8206 on 2024-08-15 17:16_

I think the status quo is that we haven't shown anything regarding the lock without verbose enabled, so let's continue with that for now. It seems easy to adjust in response to user feedback and I think a decision will be more obvious once we play with these changes for a bit longer.

---

_Merged by @charliermarsh on 2024-08-15 17:19_

---

_Closed by @charliermarsh on 2024-08-15 17:19_

---

_Branch deleted on 2024-08-15 17:19_

---
