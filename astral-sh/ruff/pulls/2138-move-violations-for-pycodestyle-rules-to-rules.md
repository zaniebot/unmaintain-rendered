```yaml
number: 2138
title: Move violations for pycodestyle rules to rules files
type: pull_request
state: merged
author: ericroberts
labels: []
assignees: []
merged: true
base: main
head: pycode-style-move-violations-to-rules
created_at: 2023-01-24T17:43:38Z
updated_at: 2023-01-26T01:18:58Z
url: https://github.com/astral-sh/ruff/pull/2138
synced_at: 2026-01-12T04:52:00Z
```

# Move violations for pycodestyle rules to rules files

---

_Pull request opened by @ericroberts on 2023-01-24 17:43_

Following up on another discussion point from https://github.com/charliermarsh/ruff/pull/2038, that violations should go together with the rules.

I noticed while doing this that some rules have multiple violations. I don't know if this will be a problem when it comes to the idea regarding auto generating documentation. Should there be a 1:1 rule:violation mapping?

I also notice in the registry that there are things under pycodestyle errors but they're not rules in the pycodestyle module. Haven't looked into it yet to determine if they should be.

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/mod.rs`:8 on 2023-01-24 17:44_

Realized this is actually a helper and opened a PR to fix that: https://github.com/charliermarsh/ruff/pull/2137

---

_@ericroberts reviewed on 2023-01-24 17:44_

---

_Comment by @charliermarsh on 2023-01-25 23:06_

@ericroberts - Tried to rebase this but I think I may not have edit rights on the PR. Do you mind either rebasing, or giving me edit rights?

---

_Comment by @ericroberts on 2023-01-26 01:02_

rebased!

---

_Merged by @charliermarsh on 2023-01-26 01:11_

---

_Closed by @charliermarsh on 2023-01-26 01:11_

---

_Comment by @charliermarsh on 2023-01-26 01:11_

Thanks!

---

_Branch deleted on 2023-01-26 01:18_

---
