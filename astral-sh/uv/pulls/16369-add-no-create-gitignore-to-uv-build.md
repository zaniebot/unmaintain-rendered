```yaml
number: 16369
title: "Add `--no-create-gitignore` to `uv build`"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/build-no-gitignore
created_at: 2025-10-20T09:34:13Z
updated_at: 2025-10-28T12:25:33Z
url: https://github.com/astral-sh/uv/pull/16369
synced_at: 2026-01-12T16:12:14Z
```

# Add `--no-create-gitignore` to `uv build`

---

_@konstin_

Fixes #16332


---

_Review requested from @zanieb by @konstin on 2025-10-20 09:34_

---

_Label `enhancement` added by @konstin on 2025-10-20 09:34_

---

_@zanieb reviewed on 2025-10-23 20:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2641 on 2025-10-23 20:10_

I usually add a second paragraph explaining why we do this by default, e.g.,

> By default, uv will create a `.gitignore` file in the output directory to ...
> When this flag is used, the file will be omitted.

---

_@zanieb reviewed on 2025-10-23 20:10_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2641 on 2025-10-23 20:10_

(I think these tend to end in periods?)

---

_@zanieb reviewed on 2025-10-23 20:11_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2643 on 2025-10-23 20:11_

My intuition when I saw this flag is that it would toggle whether or not gitignore is respected during the build. I think this could be confusing enough (and need for this option seems rare enough) that I would call this `--no-create-gitignore` / `--create-gitignore`.

---

_Renamed from "Add `--no-gitignore` to `uv build`" to "Add `--no-create-gitignore` to `uv build`" by @konstin on 2025-10-27 18:16_

---

_@zanieb approved on 2025-10-27 19:29_

---

_@zanieb reviewed on 2025-10-27 19:30_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2645 on 2025-10-27 19:30_

> only the build artifacts (source distributions and wheels) are stored in the output directory.

This part isn't particularly future-proof. I'd probably reference the use-case instead?

> When this flag is used, the file will be omitted, e.g., for use-cases where the artifacts
> need to be checked in to version control.

---

_@konstin reviewed on 2025-10-28 08:00_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2645 on 2025-10-28 08:00_

I chose the shorter version, there's other use cases e.g. building a find links index where you don't want a gitignore but it's also not in Git.

---

_Merged by @zanieb on 2025-10-28 12:25_

---

_Closed by @zanieb on 2025-10-28 12:25_

---

_Branch deleted on 2025-10-28 12:25_

---
