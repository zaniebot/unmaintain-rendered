```yaml
number: 8269
title: Add a FreeBSD build to CI
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/freebsd
created_at: 2024-10-16T19:34:47Z
updated_at: 2024-10-17T15:24:23Z
url: https://github.com/astral-sh/uv/pull/8269
synced_at: 2026-01-12T16:08:14Z
```

# Add a FreeBSD build to CI

---

_@zanieb_

Playing with this because it's interesting and I learned about this cool firecracker action.

Related #3370 

---

_Label `testing` added by @zanieb on 2024-10-16 19:34_

---

_@zanieb reviewed on 2024-10-17 14:21_

---

_Review comment by @zanieb on `crates/uv-performance-flate2-backend/Cargo.toml`:9 on 2024-10-17 14:21_

This is a real behavioral change. I'm not sure if it's needed unless you're cross-building like I am? Seems fine though? Should I split into a separate PR for changelog?

---

_Marked ready for review by @zanieb on 2024-10-17 14:21_

---

_Review requested from @konstin by @zanieb on 2024-10-17 14:39_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:587 on 2024-10-17 14:43_

Can you add a comment why we're listing more than one path here? It's not apparent to me from the rsync man page

---

_@konstin approved on 2024-10-17 14:44_

That firefox vm is cool

---

_@zanieb reviewed on 2024-10-17 15:09_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:587 on 2024-10-17 15:09_

I don't understand either. I had to copy this from rbspy — it didn't work with just the last path. I guess the exclude `*` overrides the includes unless we provide each directory?

---

_@zanieb reviewed on 2024-10-17 15:16_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:587 on 2024-10-17 15:16_

That is it https://stackoverflow.com/questions/9952000/using-rsync-include-and-exclude-options-to-include-directory-and-file-by-pattern

---

_Review comment by @konstin on `.github/workflows/ci.yml`:587 on 2024-10-17 15:19_

just that link as inline comment would be helpful for when we need to touch that job again

---

_@konstin reviewed on 2024-10-17 15:19_

---

_Merged by @zanieb on 2024-10-17 15:24_

---

_Closed by @zanieb on 2024-10-17 15:24_

---

_Branch deleted on 2024-10-17 15:24_

---
