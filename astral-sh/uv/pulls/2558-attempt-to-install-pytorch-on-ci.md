```yaml
number: 2558
title: Attempt to install PyTorch on CI
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/stream-error
created_at: 2024-03-20T01:06:32Z
updated_at: 2024-04-24T19:59:32Z
url: https://github.com/astral-sh/uv/pull/2558
synced_at: 2026-01-12T16:05:06Z
```

# Attempt to install PyTorch on CI

---

_@charliermarsh_

Trying to reproduce reported errors.

---

_Comment by @charliermarsh on 2024-03-20 03:33_

@konstin - did you do anything special to reproduce this?

---

_Comment by @janosh on 2024-03-20 10:46_

maybe try with a different Python version (e.g. 3.9) and into `--system` Python and with cache.

at least that's the setup we have over at https://github.com/materialsproject/pymatgen and we used to get time outs until [this workaround](https://github.com/materialsproject/pymatgen/commit/0ab8707aa1ef5db1341d1c0429962fff3c560d19#diff-faff1af3d8ff408964a57b2e475f69a6b7c7b71c9978cccc8f471798caac2c88R105)

---

_Comment by @konstin on 2024-03-20 11:04_

I only had them on CI, e.g. https://github.com/konstin/vws-python-mock/actions/runs/8081842244/job/22081322082 . They are still rare, even on CI with 50 jobs it often passes.

---

_Comment by @adamtheturtle on 2024-03-22 08:30_

> I only had them on CI, e.g. https://github.com/konstin/vws-python-mock/actions/runs/8081842244/job/22081322082 . They are still rare, even on CI with 50 jobs it often passes.

Thank you @konstin for looking into this.
I run a few hundred jobs per day and I hit this on average more than once per day, often in clusters (e.g. this run https://github.com/VWS-Python/vws-python-mock/actions/runs/8340991068 has 79 jobs with 24 failures).

---

_Closed by @charliermarsh on 2024-04-24 19:59_

---
