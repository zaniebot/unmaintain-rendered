```yaml
number: 2183
title: "flake8_executable: Only match shebang at beginning of line"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: shebang-anchor
created_at: 2023-01-25T23:46:06Z
updated_at: 2023-01-26T00:06:35Z
url: https://github.com/astral-sh/ruff/pull/2183
synced_at: 2026-01-12T04:52:00Z
```

# flake8_executable: Only match shebang at beginning of line

---

_Pull request opened by @andersk on 2023-01-25 23:46_

The Python implementation [uses `re.match`](https://github.com/xuhdev/flake8-executable/blob/v2.1.3/flake8_executable/__init__.py#L124) for this, which only matches at the beginning of a line.

We could use `Regex::captures_read_at`, but that’s a more complicated API; it’s easier to anchor the regex with `^`.

(Cc @sbrugman)

---

_Merged by @charliermarsh on 2023-01-25 23:57_

---

_Closed by @charliermarsh on 2023-01-25 23:57_

---

_Branch deleted on 2023-01-26 00:06_

---
