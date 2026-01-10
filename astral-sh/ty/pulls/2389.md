```yaml
number: 2389
title: Set the manylinux tag explicitly for ppc64 builds
type: pull_request
state: open
author: zanieb
labels: []
assignees: []
base: main
head: zb/ppc-force
created_at: 2026-01-07T23:32:03Z
updated_at: 2026-01-08T13:05:11Z
url: https://github.com/astral-sh/ty/pull/2389
synced_at: 2026-01-10T02:34:11Z
```

# Set the manylinux tag explicitly for ppc64 builds

---

_Pull request opened by @zanieb on 2026-01-07 23:32_

As of the latest version of maturin, this actually may not be compatible with the manylinux tag, but I think we should continue publishing it until we've made the actual choice to drop support. This is redundant with #2387 which downgrades maturin, but relevant for future upgrades.

---

_Marked ready for review by @zanieb on 2026-01-07 23:41_

---

_Comment by @charliermarsh on 2026-01-08 00:01_

Does this _fail_ on the latest maturin version? (Like, requesting a tag that maturin views as incompatible?)

---

_Comment by @zanieb on 2026-01-08 02:25_

I don't think so, but I could put up a branch on top to check?

---

_Comment by @zanieb on 2026-01-08 13:05_

Superseded by https://github.com/astral-sh/ty/pull/2393

---
