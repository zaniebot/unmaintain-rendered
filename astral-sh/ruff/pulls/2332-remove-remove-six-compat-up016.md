```yaml
number: 2332
title: Remove remove-six-compat (UP016)
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up016
created_at: 2023-01-30T00:26:41Z
updated_at: 2023-01-31T17:55:01Z
url: https://github.com/astral-sh/ruff/pull/2332
synced_at: 2026-01-12T04:52:00Z
```

# Remove remove-six-compat (UP016)

---

_Pull request opened by @charliermarsh on 2023-01-30 00:26_

See discussion in #2049.

---

_Comment by @colin99d on 2023-01-30 02:16_

I have this in my PR too.

---

_Comment by @charliermarsh on 2023-01-30 02:17_

In which PR?

---

_Merged by @charliermarsh on 2023-01-30 02:19_

---

_Closed by @charliermarsh on 2023-01-30 02:19_

---

_Branch deleted on 2023-01-30 02:20_

---

_Comment by @charliermarsh on 2023-01-30 02:20_

(Let's rebase it, I'd like this to be a separate PR.)

---

_Comment by @spaceone on 2023-01-31 17:51_

:-( it was and will be useful for me in the future (we can drop Py2 next year)

---

_Comment by @charliermarsh on 2023-01-31 17:55_

@spaceone - Sorry about that :( I recommend using `pyupgrade` for this check. I felt like the implementation in Ruff was complex and under-tested since it's a rarely-run rule.

---
