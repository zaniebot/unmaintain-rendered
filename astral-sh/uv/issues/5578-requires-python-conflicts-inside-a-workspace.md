```yaml
number: 5578
title: Requires-python conflicts inside a workspace
type: issue
state: closed
author: bluss
labels:
  - needs-decision
  - preview
assignees: []
created_at: 2024-07-29T22:52:45Z
updated_at: 2024-07-31T16:08:54Z
url: https://github.com/astral-sh/uv/issues/5578
synced_at: 2026-01-10T04:53:49Z
```

# Requires-python conflicts inside a workspace

---

_Issue opened by @bluss on 2024-07-29 22:52_

Workspaces seem to require that all members use the same requires-python. Maybe this can be relaxed, or a dedicated error can explain it.

Steps to reproduce

```shell
uv init --virtual root
cd root
uv init -p 3.11 a
uv init -p 3.12 b
uv sync
```

It should have set up two projects a, b in the same workspace with conflicting requires-python of \>= 3.11 and \>= 3.12.

### Actual Behaviour

```
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.11) does not satisfy Python>=3.12 and
      b==0.1.0 depends on Python>=3.12, we can conclude that b==0.1.0 cannot be used.
      And because only b==0.1.0 is available and you require b, we can conclude that the
      requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.11) includes Python versions that are not
      supported by your dependencies (e.g., b==0.1.0 only supports >=3.12). Consider using
      a more restrictive `requires-python` value (like >=3.12).
```

### Expected Behaviour

Logically, the combination of a and b is fine to use with any python >= 3.12. There would be a reason to emit error is if **a** depends directly on **b**.

This situation seems to happen quite easily. Not sure what's the best way to improve.


uv 0.2.31
platform: linux (x86_64)

---

_Comment by @charliermarsh on 2024-07-29 23:12_

Right now we take the union of the versions. I think what you're suggesting is we could take the intersection...

---

_Comment by @charliermarsh on 2024-07-30 02:44_

Thinking on this more, I think intersection is probably right here (as long as we reject trying to install at a lower version).

---

_Label `needs-decision` added by @charliermarsh on 2024-07-30 12:25_

---

_Label `preview` added by @charliermarsh on 2024-07-30 12:25_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 16:07_

---

_Closed by @charliermarsh on 2024-07-31 16:08_

---

_Closed by @charliermarsh on 2024-07-31 16:08_

---
