```yaml
number: 10875
title: relax error checking around unconditional enabling of conflicting extras
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/unconditional-extra-conflict
created_at: 2025-01-22T19:55:07Z
updated_at: 2025-01-22T23:52:06Z
url: https://github.com/astral-sh/uv/pull/10875
synced_at: 2026-01-12T16:09:33Z
```

# relax error checking around unconditional enabling of conflicting extras

---

_@BurntSushi_

When I first added conflicting extras, I made `package[foo]` a hard
error during resolution whenever `foo` was declared as a conflicting
extra. This was a conservative approach meant to prevent cases where
conflicts occurred non-locally throughout the dependency tree.

However, as #9942 and #10590 seem to suggest, this ends up making
some use cases impossible. For example, it prevents the ability
to encapsulate the conflicting extras of a package from a parent
dependent. This overall makes conflicting extras much less flexible.

So in this PR, we remove that error checking. But doing so ends up
making it possible to install multiple versions of the same package
into the same environment. So we "recapture" the error check at
installation time instead of resolution time. This affords us more
flexibility, but it now means that we can generate lock files that will
*always* fail to install.

This is best reviewed commit-by-commit.

Fixes #9942, Fixes #10590


---

_Review requested from @konstin by @BurntSushi on 2025-01-22 20:01_

---

_Review request for @konstin removed by @BurntSushi on 2025-01-22 20:01_

---

_Review requested from @charliermarsh by @BurntSushi on 2025-01-22 20:01_

---

_Review requested from @konstin by @BurntSushi on 2025-01-22 20:01_

---

_@charliermarsh approved on 2025-01-22 20:41_

---

_Merged by @BurntSushi on 2025-01-22 23:52_

---

_Closed by @BurntSushi on 2025-01-22 23:52_

---

_Branch deleted on 2025-01-22 23:52_

---
