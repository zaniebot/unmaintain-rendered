```yaml
number: 11072
title: "Update `uv python install --reinstall` to reinstall all previous versions"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/reinstall-all
created_at: 2025-01-29T17:29:43Z
updated_at: 2025-02-02T17:56:28Z
url: https://github.com/astral-sh/uv/pull/11072
synced_at: 2026-01-10T11:10:34Z
```

# Update `uv python install --reinstall` to reinstall all previous versions

---

_Pull request opened by @zanieb on 2025-01-29 17:29_

Since we're shipping substantive updates to Python versions frequently, I want to lower the bar for reinstalling with the latest distributions.

There's a follow-up task that's documented in a test case at https://github.com/astral-sh/uv/pull/11072/files#diff-f499c776e1d8cc5e55d7620786e32e8732b675abd98e246c0971130f5de9ed50R157-R158

---

_Label `enhancement` added by @zanieb on 2025-01-29 17:29_

---

_@charliermarsh reviewed on 2025-01-30 01:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:233 on 2025-01-30 01:28_

The indentation is off here.

---

_@charliermarsh reviewed on 2025-01-30 01:29_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:238 on 2025-01-30 01:29_

Wait, what is this case?

---

_@zanieb reviewed on 2025-01-30 01:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:238 on 2025-01-30 01:32_

It's the one in this test case https://github.com/astral-sh/uv/pull/11072/files#diff-f499c776e1d8cc5e55d7620786e32e8732b675abd98e246c0971130f5de9ed50R157-R158

---

_@zanieb reviewed on 2025-01-30 01:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:238 on 2025-01-30 01:35_

We tell people to upgrade to the latest patch with `uv python install 3.13 --reinstall` so we can't change that behavior yet. Ideally that would _not_ change your patch version and you'd use `uv python install 3.13 --upgrade` to do that instead.

---

_@charliermarsh approved on 2025-01-30 14:06_

---

_@charliermarsh reviewed on 2025-01-30 14:06_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:238 on 2025-01-30 14:06_

Ok, so the `uv python install 3.13 --reinstall` behavior is unchanged, right? And we'll uninstall the existing `3.13`?

---

_@zanieb reviewed on 2025-01-30 14:25_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:238 on 2025-01-30 14:25_

Yep exactly. It's only if you do `uv python install any --reinstall` or `uv python install --reinstall` (and don't have a `.python-version` file) that this new behavior takes place.

---

_Merged by @zanieb on 2025-01-30 16:08_

---

_Closed by @zanieb on 2025-01-30 16:08_

---

_Branch deleted on 2025-01-30 16:08_

---
