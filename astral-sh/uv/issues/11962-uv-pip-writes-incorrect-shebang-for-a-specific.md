```yaml
number: 11962
title: uv pip writes incorrect shebang for a specific project
type: issue
state: closed
author: mistydemeo
labels:
  - bug
  - external
assignees: []
created_at: 2025-03-04T23:42:46Z
updated_at: 2025-03-05T00:08:13Z
url: https://github.com/astral-sh/uv/issues/11962
synced_at: 2026-01-12T16:00:50Z
```

# uv pip writes incorrect shebang for a specific project

---

_@mistydemeo_

### Summary

I've noticed that, for one specific project I have, `uv pip install -e .` is writing out incorrect shebangs for its scripts. Instead of writing shebangs using paths from within the venv, it's writing out paths from UV's build cache - which don't exist anymore, and can't be run.

Steps to repro:

1. Clone this repository: https://github.com/mistydemeo/umbra
2. Check out the branch `misty/python-12`
3. Run `uv venv`
4. Run `uv pip install -e .`
5. Check the shebang on `.venv/bin/umbra`

Expected:

The shebang reads `#!$HOME/git/umbra/.venv/bin/python3`

Actual:

The shebang reads `#!$HOME/.cache/uv/builds-v0/.tmpQMKYdQ/bin/python`

### Platform

macOS 15 arm64

### Version

uv 0.6.4 (04db70662 2025-03-03)

### Python version

Python 3.12.8

---

_Label `bug` added by @mistydemeo on 2025-03-04 23:42_

---

_Comment by @charliermarsh on 2025-03-05 00:04_

I think this might be https://github.com/astral-sh/uv/issues/2744.

---

_Comment by @zanieb on 2025-03-05 00:04_

Without digging deeply here, I worry this is because `scripts` is using a glob https://github.com/mistydemeo/umbra/blob/1a9a39606856b30f17885518dba8436c8909e0a2/setup.py#L15 and that this is a `setuptools` specific problem.

If editable installs of entry points were generally broken, we'd have more reports.

---

_Comment by @zanieb on 2025-03-05 00:05_

Looks like the issue Charlie linked to confirms that. I think this needs to be fixed upstream in setuptools.

---

_Label `external` added by @zanieb on 2025-03-05 00:05_

---

_Comment by @mistydemeo on 2025-03-05 00:06_

You're right, this looks like the same issue. Sorry about that, I'd searched but hadn't found it. I can close this in favour of that issue.

---

_Comment by @zanieb on 2025-03-05 00:07_

No problem

---

_Closed by @zanieb on 2025-03-05 00:07_

---

_Comment by @charliermarsh on 2025-03-05 00:07_

All good, I wouldn't expect anyone to connect the dots on these issues, the issue tracker history is just buried deep in my brain so I might as well use that knowledge for good.

---

_Comment by @zanieb on 2025-03-05 00:08_

I changed the title a little to try to help with discovery.

---
