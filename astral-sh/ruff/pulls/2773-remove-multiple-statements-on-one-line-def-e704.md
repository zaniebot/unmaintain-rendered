```yaml
number: 2773
title: Remove multiple-statements-on-one-line-def (E704)
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2023-02-11T17:33:03Z
updated_at: 2025-04-07T14:27:36Z
url: https://github.com/astral-sh/ruff/pull/2773
synced_at: 2026-01-10T19:40:36Z
```

# Remove multiple-statements-on-one-line-def (E704)

---

_Pull request opened by @charliermarsh on 2023-02-11 17:33_

When I implemented this, I didn't realize that it's off-by-default in pycodestyle and Flake8, because they are "not rules unanimously accepted, and [PEP 8](http://www.python.org/dev/peps/pep-0008/) does not enforce them". Rather than opt-in to the complexity of maintaining an ignore list, I'd rather just omit those rules entirely.

See: https://pycodestyle.pycqa.org/en/latest/intro.html.

Closes #2763.


---

_Label `breaking` added by @charliermarsh on 2023-02-11 17:34_

---

_Merged by @charliermarsh on 2023-02-11 17:34_

---

_Closed by @charliermarsh on 2023-02-11 17:34_

---

_Branch deleted on 2023-02-11 17:34_

---

_Comment by @not-my-profile on 2023-02-11 17:43_

You forgot to run `cargo dev generate-all`, see the CI failure on main.

---

_Comment by @spaceone on 2025-04-07 14:27_

Wouldn't it be an alternative to not remove the rule but disable it by default? I find it useful, nevertheless if it's part of PEP8 or not.

---
