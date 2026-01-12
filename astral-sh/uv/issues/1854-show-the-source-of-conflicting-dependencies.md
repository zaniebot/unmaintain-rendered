```yaml
number: 1854
title: Show the source of conflicting dependencies
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-02-22T04:20:07Z
updated_at: 2024-08-02T22:13:32Z
url: https://github.com/astral-sh/uv/issues/1854
synced_at: 2026-01-12T15:58:32Z
```

# Show the source of conflicting dependencies

---

_@zanieb_

When dependencies directly conflict, we should show which file we loaded them from.

e.g.

```
× No solution found when resolving dependencies:
╰─▶ Because you require filelock==1.0.0 and you require filelock==3.8.0, we
    can conclude that the requirements are unsatisfiable.
```

from `compile_constraints_incompatible_version` could say

```
× No solution found when resolving dependencies:
╰─▶ Because you require filelock==1.0.0 (from requirements.txt) and you require filelock==3.8.0 (from constraints.txt), we
    can conclude that the requirements are unsatisfiable.
```


_Originally mentioned at https://github.com/astral-sh/uv/pull/1796/files#r1498258800_

---

_Label `error messages` added by @zanieb on 2024-02-22 04:21_

---

_Comment by @telamonian on 2024-08-02 22:13_

@zanieb I need this functionality for for an app installer tool that I'm writing. If you're willing to point out the files and rough line numbers that need to change, I would be happy to contribute a fix for this.

---
