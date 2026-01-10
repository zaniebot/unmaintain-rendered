```yaml
number: 17220
title: Use nextest profiles to configure CI
type: pull_request
state: merged
author: EliteTK
labels:
  - internal
  - "test:macos"
assignees: []
merged: true
base: main
head: tk/ci-profiles
created_at: 2025-12-22T20:20:32Z
updated_at: 2025-12-29T13:13:46Z
url: https://github.com/astral-sh/uv/pull/17220
synced_at: 2026-01-10T05:49:14Z
```

# Use nextest profiles to configure CI

---

_Pull request opened by @EliteTK on 2025-12-22 20:20_

This centralises the configuration and allows certain overrides to be CI specific.

---

_Label `internal` added by @EliteTK on 2025-12-22 20:20_

---

_Label `no-test` added by @EliteTK on 2025-12-22 20:20_

---

_Label `no-test` removed by @EliteTK on 2025-12-22 20:20_

---

_Label `test:macos` added by @EliteTK on 2025-12-22 20:42_

---

_Comment by @EliteTK on 2025-12-22 21:01_

This is really for #17177. I wasn't sure if it was okay to just bump the nextest version to 0.9.115 so for now I have just added a commit which manually expands all the profiles.

---

_Marked ready for review by @EliteTK on 2025-12-22 21:03_

---

_Review requested from @zanieb by @EliteTK on 2025-12-22 21:03_

---

_@zanieb approved on 2025-12-22 21:39_

Do they support layering profiles? i.e., can we inherit most of these ci settings from a single profile?

---

_Comment by @EliteTK on 2025-12-22 21:41_

Yes they do support layering but the version of nextest we're using doesn't support it. I wasn't sure if it was okay to bump it so I added the commit manually un-layering these.

---

_Comment by @zanieb on 2025-12-22 21:56_

It's fine to bump the version imo as long as you audited it for anything malicious.

---

_Comment by @EliteTK on 2025-12-22 23:41_

Looks like I'd need to update the taiki-e/install-action to do that. I'll maybe just create an issue and get on it next week and merge this as is (if the git-lfs tests ever succeed)...

---

_Merged by @EliteTK on 2025-12-29 13:13_

---

_Closed by @EliteTK on 2025-12-29 13:13_

---

_Branch deleted on 2025-12-29 13:13_

---
