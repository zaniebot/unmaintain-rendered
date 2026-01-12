```yaml
number: 1275
title: "pygrep-hooks - deprecated use of logging.warn & no blanket type ignore"
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: pygrep-hooks
created_at: 2022-12-18T06:44:30Z
updated_at: 2022-12-19T04:22:16Z
url: https://github.com/astral-sh/ruff/pull/1275
synced_at: 2026-01-12T15:55:06Z
```

# pygrep-hooks - deprecated use of logging.warn & no blanket type ignore

---

_@squiddy_

_No description provided._

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:40 on 2022-12-18 19:28_

I think we should _either_ only run this on lines with comments (`directives.commented_lines`), or move this into `checkers/tokens.rs` and run it directly on `Tok::Comment` nodes (the downside of which is that we have to use `Locator` to get the comment).

---

_@charliermarsh reviewed on 2022-12-18 19:28_

---

_Merged by @charliermarsh on 2022-12-18 23:04_

---

_Closed by @charliermarsh on 2022-12-18 23:04_

---

_Branch deleted on 2022-12-19 04:22_

---
