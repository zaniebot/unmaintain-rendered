```yaml
number: 952
title: Allow long lines that consist of only a URL
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/url
created_at: 2022-11-29T01:10:18Z
updated_at: 2022-11-29T01:10:22Z
url: https://github.com/astral-sh/ruff/pull/952
synced_at: 2026-01-12T15:55:05Z
```

# Allow long lines that consist of only a URL

---

_@charliermarsh_

In #920, we changed the line-length detection to allow long lines to end with a URL. However, we introduced a bug for the case in which a comment is _only_ a URL, since `chunks.last` would return `None`, which we defaulted to `false`.

Resolves #950.

---

_Merged by @charliermarsh on 2022-11-29 01:10_

---

_Closed by @charliermarsh on 2022-11-29 01:10_

---

_Branch deleted on 2022-11-29 01:10_

---
