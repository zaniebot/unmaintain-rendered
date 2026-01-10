```yaml
number: 2063
title: uv and pip do not have the same behavior when pinning a constraint to a prerelease
type: issue
state: closed
author: notatallshaw
labels:
  - bug
assignees: []
created_at: 2024-02-29T00:27:22Z
updated_at: 2024-02-29T01:16:25Z
url: https://github.com/astral-sh/uv/issues/2063
synced_at: 2026-01-10T05:40:32Z
```

# uv and pip do not have the same behavior when pinning a constraint to a prerelease

---

_Issue opened by @notatallshaw on 2024-02-29 00:27_

Environment: Linux Python 3.11.6 pip 24.0 uv 0.1.12

Constraints file `constraints.txt`:

```
exceptiongroup==1.0.0rc8
```

uv by default fails to install the requirement `exceptiongroup` with this  constraint:

```
$ echo "exceptiongroup" | uv pip compile - --constraint constraints.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of exceptiongroup==1.0.0rc8 and you require exceptiongroup==1.0.0rc8, we can conclude that the requirements
      are unsatisfiable.

      hint: exceptiongroup was requested with a pre-release marker (e.g., exceptiongroup==1.0.0rc8), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```

Where as pip succeeds:

```
$ pip install --dry-run exceptiongroup --constraint constraints.txt
Collecting exceptiongroup
  Using cached exceptiongroup-1.0.0rc8-py3-none-any.whl.metadata (4.8 kB)
Using cached exceptiongroup-1.0.0rc8-py3-none-any.whl (11 kB)
Would install exceptiongroup-1.0.0rc8
```


There's no spec on this because constraints of an installer feature of pip, not defined by a spec. However I would interpret a user specifically pinning a prerelease, even if it's in the constraints file, to mean that a prerelease should be selected. It would be good to know if this is not the case.


---

_Comment by @charliermarsh on 2024-02-29 00:30_

I think this should be allowed. I consider it a bug.

---

_Label `bug` added by @charliermarsh on 2024-02-29 00:30_

---

_Comment by @charliermarsh on 2024-02-29 00:30_

Should be an easy fix.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 00:45_

---

_Closed by @charliermarsh on 2024-02-29 01:16_

---
