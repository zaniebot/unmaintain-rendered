```yaml
number: 1556
title: "Fix `__init__.py` being private"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: fix-__init__.py-being-private
created_at: 2023-01-02T14:20:53Z
updated_at: 2023-01-02T19:39:24Z
url: https://github.com/astral-sh/ruff/pull/1556
synced_at: 2026-01-12T05:36:31Z
```

# Fix `__init__.py` being private

---

_Pull request opened by @not-my-profile on 2023-01-02 14:20_

Previously `visibility::module_visibility()` returned `Private`
for any module name starting with an underscore, resulting in
`__init__.py` being categorized as private, which in turn resulted
in D104 (Missing docstring in public package) never being reported
for `__init__.py` files.

The commits implement separate changes, so I'd rather have you not squash them.

---

_Comment by @charliermarsh on 2023-01-02 17:37_

Do you mind expanding the PR summary a bit to flesh out what changed here and why?

---

_Comment by @not-my-profile on 2023-01-02 17:47_

But of course :) I amended the commit messages and the PR description.

---

_Comment by @charliermarsh on 2023-01-02 17:49_

Thank you :)

---

_Merged by @charliermarsh on 2023-01-02 19:39_

---

_Closed by @charliermarsh on 2023-01-02 19:39_

---
