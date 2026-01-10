```yaml
number: 1233
title: "Invert display of \"no versions\" incompatibilities with multiple ranges"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/invert
created_at: 2024-02-02T17:28:00Z
updated_at: 2024-02-02T18:00:26Z
url: https://github.com/astral-sh/uv/pull/1233
synced_at: 2026-01-10T15:33:24Z
```

# Invert display of "no versions" incompatibilities with multiple ranges

---

_Pull request opened by @zanieb on 2024-02-02 17:28_

Closes #884 

e.g.

```
❯ cargo run -q -- pip compile --python-version 3.12 requirements.in
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (3.12) does not satisfy Python>=3.6,<3.10 and recommenders==1.0.0 depends on Python>=3.6,<3.9, we can conclude that recommenders==1.0.0 cannot be used.
      And because only the following versions of recommenders are available:
          recommenders<=0.7
          recommenders==1.0.0
          recommenders==1.1.0
          recommenders==1.1.1
      we can conclude that recommenders>0.7,<1.1.0 cannot be used. (1)

      Because the requested Python version (3.12) does not satisfy Python>=3.6,<3.10 and recommenders>=1.1.0 depends on Python>=3.6,<3.10, we can conclude that recommenders>=1.1.0 cannot be used.
      And because we know from (1) that recommenders>0.7,<1.1.0 cannot be used, we can conclude that recommenders>0.7 cannot be used.
      And because you require recommenders>0.7, we can conclude that the requirements are unsatisfiable.
```

---

_@konstin approved on 2024-02-02 17:30_

---

_Label `error messages` added by @zanieb on 2024-02-02 17:34_

---

_Comment by @zanieb on 2024-02-02 18:00_

I don't love this, but it's an improvement. I'll follow with more.

---

_Merged by @zanieb on 2024-02-02 18:00_

---

_Closed by @zanieb on 2024-02-02 18:00_

---

_Branch deleted on 2024-02-02 18:00_

---
