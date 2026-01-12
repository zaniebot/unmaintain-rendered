```yaml
number: 1173
title: Mark redefined-but-unused imports as unused regardless of scope
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unused-import-when-redefined
created_at: 2022-12-10T04:17:16Z
updated_at: 2022-12-10T04:17:34Z
url: https://github.com/astral-sh/ruff/pull/1173
synced_at: 2026-01-12T15:55:05Z
```

# Mark redefined-but-unused imports as unused regardless of scope

---

_@charliermarsh_

Right now, if an import is redefined-while-unused by a variable in the same scope, we lose that binding, and thus never report it as unused. This matches the current Pyflakes behavior, but... I think it can be improved?

For example, right now, these return different error combinations:

```py
# Only reports redefined-while-unused (not unused import).
import fu
def fu():
    pass

# Reports both redefined-while-unused _and_ unused import.
import fu
def bar():
    def baz():
        def fu():
            pass
```

The solution I opted for here was to preserve references to the "overridden" bindings, and include those when checking for unused imports.


---

_Merged by @charliermarsh on 2022-12-10 04:17_

---

_Closed by @charliermarsh on 2022-12-10 04:17_

---

_Branch deleted on 2022-12-10 04:17_

---
