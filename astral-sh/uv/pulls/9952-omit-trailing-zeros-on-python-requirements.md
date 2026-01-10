```yaml
number: 9952
title: Omit trailing zeros on Python requirements inferred from versions
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/requires-python-zero
created_at: 2024-12-17T03:27:49Z
updated_at: 2024-12-17T14:56:58Z
url: https://github.com/astral-sh/uv/pull/9952
synced_at: 2026-01-10T12:00:01Z
```

# Omit trailing zeros on Python requirements inferred from versions

---

_Pull request opened by @zanieb on 2024-12-17 03:27_

In a message like

```
❯ echo "numpy>2" | uv pip compile -p 3.8 -
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.8.0) does not satisfy Python>=3.10 and the requested 
  Python version (>=3.8.0) does not satisfy Python>=3.9,<3.10, we can conclude that Python>=3.9 is incompatible.
      And because numpy>=2.0.1,<=2.0.2 depends on Python>=3.9 and only the following versions of numpy are available:
          numpy<=2.0.2
```

I'm surprised that `-p 3.8` leads to expressions like `>=3.8.0` (I understand it, of course, but it's not intuitive) and then all the _other_ Python versions in the message omit the trailing zero. This updates the `PythonRequirement` parsing to drop the trailing zeros. It's easier to do there because the version is not yet abstracted.

---

_Marked ready for review by @zanieb on 2024-12-17 04:37_

---

_Review requested from @BurntSushi by @zanieb on 2024-12-17 04:37_

---

_Review requested from @charliermarsh by @zanieb on 2024-12-17 04:37_

---

_@charliermarsh approved on 2024-12-17 13:38_

---

_Label `error messages` added by @zanieb on 2024-12-17 14:18_

---

_Merged by @zanieb on 2024-12-17 14:18_

---

_Closed by @zanieb on 2024-12-17 14:18_

---

_Branch deleted on 2024-12-17 14:18_

---

_@BurntSushi reviewed on 2024-12-17 14:49_

Makes sense to me!

---

_Comment by @zanieb on 2024-12-17 14:56_

Glad my trailing zero removal isn't too unsound :)

---
