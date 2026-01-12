```yaml
number: 9234
title: The Cargo.lock file is out of sync in the v0.1.9 release
type: issue
state: closed
author: alerque
labels: []
assignees: []
created_at: 2023-12-21T19:31:08Z
updated_at: 2023-12-21T20:51:15Z
url: https://github.com/astral-sh/ruff/issues/9234
synced_at: 2026-01-12T15:54:49Z
```

# The Cargo.lock file is out of sync in the v0.1.9 release

---

_@alerque_

Arch Linux package builds for Rust projects typically use dependency pre-fetching then build in offline mode (`--frozen`). This requires the Cargo.lock file be up to date with Cargo.toml at release time otherwise the locking just throws an error. We can skip this, but it typically means either eschewing reproducibility or some elaborate process of patching the lock file in a reproducible way.

Can you please update the lock file prior to releases so they are always in sync at tagged release points?

As a side note it would be good in the CI builds used `--locked` as well to verify this and also produce more reproducible binaries.

---

_Closed by @charliermarsh on 2023-12-21 20:47_

---

_Comment by @charliermarsh on 2023-12-21 20:51_

Understood, thanks, and sorry for the trouble. We've updated our release checklist to reflect this.

---
