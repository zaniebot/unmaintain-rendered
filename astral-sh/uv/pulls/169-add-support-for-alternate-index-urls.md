```yaml
number: 169
title: Add support for alternate index URLs
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/registry-url
created_at: 2023-10-23T03:13:42Z
updated_at: 2023-10-24T07:44:44Z
url: https://github.com/astral-sh/uv/pull/169
synced_at: 2026-01-12T16:03:47Z
```

# Add support for alternate index URLs

---

_@charliermarsh_

As elsewhere, we just use the `pip` and `pip-compile` APIs. So we support `--index-url` to override PyPI, then `--extra-index-url` to add _additional_ indexes, and `--no-index` to avoid hitting the index at all.

Closes #156.

---

_Merged by @charliermarsh on 2023-10-23 03:18_

---

_Closed by @charliermarsh on 2023-10-23 03:18_

---

_Branch deleted on 2023-10-23 03:18_

---

_Comment by @konstin on 2023-10-23 06:44_

Do you want to have that support in requirements.txt, too?

---

_Comment by @charliermarsh on 2023-10-23 13:05_

We probably should, yeah.

---

_Comment by @charliermarsh on 2023-10-23 21:49_

@konstin - To support this, would we need to modify the requirements parser to include these arguments?

---

_Comment by @konstin on 2023-10-24 07:44_

yep

---
