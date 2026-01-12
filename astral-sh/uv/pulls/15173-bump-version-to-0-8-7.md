```yaml
number: 15173
title: Bump version to 0.8.7
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/087
created_at: 2025-08-08T19:03:48Z
updated_at: 2025-08-08T19:42:25Z
url: https://github.com/astral-sh/uv/pull/15173
synced_at: 2026-01-12T16:11:37Z
```

# Bump version to 0.8.7

---

_@zanieb_

_No description provided._

---

_Review comment by @geofft on `CHANGELOG.md`:10 on 2025-08-08 19:09_

Wanna add that it closes issues in this repo like #6893?

---

_Review comment by @geofft on `CHANGELOG.md`:20 on 2025-08-08 19:11_

Do you want to expand on what `find_uv_bin` is? This fixes the Python-language wrapper around uv that's in the uv wheel?

---

_Review comment by @geofft on `CHANGELOG.md`:28 on 2025-08-08 19:12_

Is it worth adding that these control whether the `dev` dependency group is used?, or is this obvious enough from the naming and the implication that they do the same thing as `--dev` and `--no-dev`?

---

_Review comment by @geofft on `CHANGELOG.md`:36 on 2025-08-08 19:14_

Wow this seems kind of major! (In a good way)

I kind of want to be a pedant and say "two distributions" but nobody knows what that means. (Perhaps "when installing a package that overwrites a file in another package"?)

---

_@geofft approved on 2025-08-08 19:14_

---

_@zanieb reviewed on 2025-08-08 19:21_

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-08-08 19:21_

We don't normally do that in the release notes, so I lean no.

---

_@zanieb reviewed on 2025-08-08 19:22_

---

_Review comment by @zanieb on `CHANGELOG.md`:20 on 2025-08-08 19:22_

I'm not sure. For the readers it's relevant for, they will know what it is I think? It's kind of funny because I think it's our only Python API?

---

_@zanieb reviewed on 2025-08-08 19:22_

---

_Review comment by @zanieb on `CHANGELOG.md`:36 on 2025-08-08 19:22_

Oops I actually meant to move this out of the bug fix section

---

_@zanieb reviewed on 2025-08-08 19:26_

---

_Review comment by @zanieb on `CHANGELOG.md`:36 on 2025-08-08 19:26_

We don't actually do per-file detection yet, just top-level modules

---

_Merged by @zanieb on 2025-08-08 19:42_

---

_Closed by @zanieb on 2025-08-08 19:42_

---

_Branch deleted on 2025-08-08 19:42_

---
