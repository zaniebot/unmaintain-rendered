```yaml
number: 221
title: Implement F402
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: F402
created_at: 2022-09-18T04:39:14Z
updated_at: 2022-09-21T15:12:55Z
url: https://github.com/astral-sh/ruff/pull/221
synced_at: 2026-01-12T05:48:45Z
```

# Implement F402

---

_Pull request opened by @harupy on 2022-09-18 04:39_

#170

```
> flake8 resources/test/fixtures/F402.py
resources/test/fixtures/F402.py:5:5: F402 import 'os' from line 1 shadowed by loop variable
resources/test/fixtures/F402.py:8:5: F402 import 'path' from line 2 shadowed by loop variable

> cargo run -- --no-cache resources/test/fixtures/F402.py
./resources/test/fixtures/F402.py:5:5: F402 import 'os' from line 1 shadowed by loop variable
./resources/test/fixtures/F402.py:8:5: F402 import 'path' from line 2 shadowed by loop variable
```

---

_@charliermarsh reviewed on 2022-09-19 19:17_

---

_Review comment by @charliermarsh on `resources/test/fixtures/F402.py`:9 on 2022-09-19 19:17_

Flake8 has this check for `self.differentForks(node, existing.source)`, which I _think_ enables them to avoid flagging errors in cases like this:

```py
if False:
    import typing
else:
    for typing in range(3):
        pass
```

(`ruff` raises F402 here, but Flake8 doesn't.)

It might be okay to have false positives for this rule in particular, but it seems like something we'll have to solve to support `RedefinedWhileUnused` and some others. `differentForks` requires that we store the parent stack for every binding, and then implement the `differentForks` logic.

Do you think that's worth doing here, as part of this PR? Or would you rather merge w/ these limitations?

(Either way, can we add an example using the `if`/`else` to `F402.py`, even if it has a TODO saying that we shouldn't be flagging that line?)


---

_Review comment by @charliermarsh on `resources/test/fixtures/F402.py`:9 on 2022-09-20 02:26_

I guess pylint doesn't really do this, so maybe we should just not worry about it for now.

For example, pylint has a rule (`W0621`) around using a variable name that shadows a variable in the outer scope, and it does get triggered here:

```py
if True:
    typing = 1
else:
    def f(typing):
        pass
```

---

_@charliermarsh reviewed on 2022-09-20 02:26_

---

_Merged by @charliermarsh on 2022-09-21 15:12_

---

_Closed by @charliermarsh on 2022-09-21 15:12_

---
