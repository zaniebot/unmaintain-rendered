```yaml
number: 2748
title: Rewrite documentation for yield-in-init
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/docs
created_at: 2023-02-10T22:17:47Z
updated_at: 2023-02-10T22:49:56Z
url: https://github.com/astral-sh/ruff/pull/2748
synced_at: 2026-01-12T04:52:01Z
```

# Rewrite documentation for yield-in-init

---

_Pull request opened by @charliermarsh on 2023-02-10 22:17_

_No description provided._

---

_Comment by @charliermarsh on 2023-02-10 22:17_

\cc @not-my-profile if you have any more feedback.

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:23 on 2023-02-10 22:25_

I think "The `__init__` method is not allowed to return a value." should be changed to:

> The `__init__` method has to return `None`.

Since I'd argue that `None` is technically also a value.

---

_@not-my-profile reviewed on 2023-02-10 22:25_

---

_@charliermarsh reviewed on 2023-02-10 22:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:23 on 2023-02-10 22:26_

Yeah I had that initially but it might imply to some that you have to _explicitly_ `return None`. Do you think it's clearer?

---

_@not-my-profile reviewed on 2023-02-10 22:33_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:23 on 2023-02-10 22:33_

Yeah I think it's clearer, quoting the [control flow page of the Python tutorial](https://docs.python.org/3/tutorial/controlflow.html):

> In fact, even functions without a [return](https://docs.python.org/3/reference/simple_stmts.html#return) statement do return a value, albeit a rather boring one. This value is called None (it’s a built-in name).

Yes this wording might be confusing for users not aware of this implicit `None` return but I think it's better to have accurate wording.

Note that the runtime error message is also e.g.

`TypeError: __init__() should return None, not 'int'`

---

_Merged by @charliermarsh on 2023-02-10 22:49_

---

_Closed by @charliermarsh on 2023-02-10 22:49_

---

_Branch deleted on 2023-02-10 22:49_

---
