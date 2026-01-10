```yaml
number: 1130
title: Add bootstrapped installation in Python for Windows
type: pull_request
state: merged
author: zanieb
labels:
  - testing
  - windows
assignees: []
merged: true
base: main
head: zb/bootstrap-python-in-python
created_at: 2024-01-26T18:09:40Z
updated_at: 2024-01-28T16:24:50Z
url: https://github.com/astral-sh/uv/pull/1130
synced_at: 2026-01-10T15:39:03Z
```

# Add bootstrapped installation in Python for Windows

---

_Pull request opened by @zanieb on 2024-01-26 18:09_

A 1:1 port of the Bash script to Python for use on Windows.

Pulls some parts of #1068 but much more minimal. Avoids an additional dependency on `requests`. Because we require `zstandard` to unzip the distributions we unfortunately cannot be dependency free and cannot have `bootstrap.sh` download the Python version needed to run this script without it doing a non-trivial amount of work.

Retains the Bash script for now so you can bootstrap without Python available. I may drop it in the future?

---

_@zanieb reviewed on 2024-01-26 18:14_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:84 on 2024-01-26 18:14_

```suggestion
      - name: "Install Python for bootstrapping"
```

---

_Label `testing` added by @zanieb on 2024-01-26 19:42_

---

_Label `windows` added by @zanieb on 2024-01-26 19:42_

---

_Marked ready for review by @zanieb on 2024-01-26 19:55_

---

_@charliermarsh approved on 2024-01-27 01:52_

I'm definitely supportive of the Python version. The Bash is impressive and nicely portable but it will be hard to maintain.

---

_@charliermarsh reviewed on 2024-01-27 01:53_

---

_Review comment by @charliermarsh on `scripts/bootstrap/install.py`:6 on 2024-01-27 01:53_

"installed" (giving myself a 🏅) 

---

_Review comment by @zanieb on `scripts/bootstrap/install.py`:6 on 2024-01-27 18:41_

```suggestion
# This script can be run without Python installed via `install.sh`
```

---

_@zanieb reviewed on 2024-01-27 18:41_

---

_Comment by @zanieb on 2024-01-27 18:42_

> I'm definitely supportive of the Python version. The Bash is impressive and nicely portable but it will be hard to maintain.

Ideally we don't maintain the Bash much and just write bootstrapping in Rust in the next month or two!

---

_Merged by @zanieb on 2024-01-28 16:24_

---

_Closed by @zanieb on 2024-01-28 16:24_

---

_Branch deleted on 2024-01-28 16:24_

---
