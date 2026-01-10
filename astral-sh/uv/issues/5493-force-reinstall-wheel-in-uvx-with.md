```yaml
number: 5493
title: Force reinstall wheel in uvx --with
type: issue
state: closed
author: bluss
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-26T20:58:08Z
updated_at: 2024-07-27T01:36:59Z
url: https://github.com/astral-sh/uv/issues/5493
synced_at: 2026-01-10T04:53:49Z
```

# Force reinstall wheel in uvx --with

---

_Issue opened by @bluss on 2024-07-26 20:58_

Here's a recipe for building a wheel and testing it in a project setup for pytest.

```
uvx --from build pyproject-build -s -w --installer uv
uvx --reinstall --refresh --with dist/*.whl pytest
```

Is there a way to force the wheel to always be reinstalled, even if name and version doesn't change? --reinstall --refresh are not enough in this particular case.

uv 0.2.30

---

_Comment by @charliermarsh on 2024-07-26 20:59_

Hmm, `--reinstall` on its own should be enough. I'll look into it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 20:59_

---

_Label `bug` added by @charliermarsh on 2024-07-26 21:03_

---

_Label `preview` added by @charliermarsh on 2024-07-26 21:03_

---

_Comment by @charliermarsh on 2024-07-26 21:03_

Yeah I consider that a bug.

---

_Comment by @charliermarsh on 2024-07-26 22:28_

There's actually a TODO for this hah.

---

_Closed by @charliermarsh on 2024-07-27 01:36_

---

_Closed by @charliermarsh on 2024-07-27 01:36_

---
