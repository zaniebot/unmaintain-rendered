```yaml
number: 1358
title: Fix false-positive in RET504 when referencing globals
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: flake8-return-globals
created_at: 2022-12-24T09:23:23Z
updated_at: 2022-12-24T18:49:00Z
url: https://github.com/astral-sh/ruff/pull/1358
synced_at: 2026-01-12T05:36:31Z
```

# Fix false-positive in RET504 when referencing globals

---

_Pull request opened by @squiddy on 2022-12-24 09:23_

Closes #1310

---

_@charliermarsh reviewed on 2022-12-24 09:39_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:1270 on 2022-12-24 09:39_

Could we pass in the list of globals, rather than relying on current_scope? That would remove the implicit dependence on the state of the scope stack when this is called.

---

_Review comment by @squiddy on `src/checkers/ast.rs`:1270 on 2022-12-24 10:30_

I was thinking of just passing `&globals` here, but I'm having some borrow-checker issues, because the globals are moved into the new scope which is taken by `push_scope`.

That said, I was just thinking that this example should also not trigger a `RET504`.

```python
def nonlocal_assignment():
    X = 1
    def inner():
        nonlocal X
        X = 1
        return X
```

So I'm wondering whether I even need to look into globals for this. Need to check the code.

---

_@squiddy reviewed on 2022-12-24 10:30_

---

_@squiddy reviewed on 2022-12-24 16:24_

---

_Review comment by @squiddy on `src/checkers/ast.rs`:1270 on 2022-12-24 16:24_

Have taken a different approach now.

---

_Merged by @charliermarsh on 2022-12-24 17:02_

---

_Closed by @charliermarsh on 2022-12-24 17:02_

---

_Branch deleted on 2022-12-24 18:49_

---
