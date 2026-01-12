```yaml
number: 5729
title: Missing file name when emiting warnings
type: issue
state: closed
author: matejsp
labels: []
assignees: []
created_at: 2023-07-13T09:00:51Z
updated_at: 2023-07-18T14:08:26Z
url: https://github.com/astral-sh/ruff/issues/5729
synced_at: 2026-01-12T15:54:45Z
```

# Missing file name when emiting warnings

---

_@matejsp_

```
(venv) ➜  rm -rf .ruff_cache && ruff .
warning: Invalid `# noqa` directive on line 6: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 1: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 7: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 3: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 25: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

This warning is emitted only on first run (after cache is build it is no longer displayed)
It would be nice to threat warnings as errors

Ruff version:
(venv) ➜ ruff --version
ruff 0.0.27

Previous version didn't emit such warnings on lines:
``` # noqa: 901 ```
now it wants complete code:
``` # noqa: C901 ```



---

_Comment by @zanieb on 2023-07-13 13:37_

@matejsp just to clarify, you're looking for three things here?

1. Include the file name in the warnings
2. Always emit warnings, even if the cache exists
3. The ability to error on warnings

Is that right?

---

_Comment by @matejsp on 2023-07-13 13:47_

1. YES (bug title)
2. would be nice
3. would also be nice :D

---

_Comment by @charliermarsh on 2023-07-13 13:49_

I think the "right" solution here might be to treat those warnings as diagnostics, rather than "warnings". As diagnostics, they would have all of those properties automatically.

---

_Comment by @charliermarsh on 2023-07-13 13:50_

It's similar to #850.

---

_Comment by @charliermarsh on 2023-07-13 13:55_

Maybe I'll rephrase that as: they should very likely be diagnostics, and not ad hoc warnings. But the downside of being diagnostics is that they won't be enabled by default for users.

---

_Closed by @charliermarsh on 2023-07-18 14:08_

---
