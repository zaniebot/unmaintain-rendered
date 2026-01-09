---
number: 14150
title: "uv pip install git+git:// is not allowed"
type: issue
state: closed
author: davidesba
labels:
  - needs-decision
assignees: []
created_at: 2025-06-20T13:05:06Z
updated_at: 2025-08-26T02:27:27Z
url: https://github.com/astral-sh/uv/issues/14150
synced_at: 2026-01-07T13:12:18-06:00
---

# uv pip install git+git:// is not allowed

---

_Issue opened by @davidesba on 2025-06-20 13:05_

### Summary

In version 0.5.3, I can install a git source like this:

```
uv pip install --python 3.11 --system "<project> @ git+git://<gitlab_server>/<project>@0.2.5"
```

This command runs this fetch command, that is allowed without needing any token or authorization:

```
/usr/bin/git fetch --force --update-head-ok 'git://<gitlab_server>/<project>' '+refs/tags/0.2.5:refs/remotes/origin/tags/0.2.5'
```

After updating to latest version, I get this error:

```
error: Failed to parse: `<project> @ git+git://<gitlab_server>/<project>@0.2.5`
  Caused by: Unsupported Git URL scheme `git:` in `git://<gitlab_server>/<project>` (expected one of `https:`, `ssh:`, or `file:`)
```

Using ssh or https would need authorization in my server


### Platform

Ubuntu 22.04.5 LTS

### Version

uv 0.7.13

### Python version

Python 3.11.9

---

_Label `bug` added by @davidesba on 2025-06-20 13:05_

---

_Label `bug` removed by @konstin on 2025-06-20 13:22_

---

_Label `enhancement` added by @konstin on 2025-06-20 13:22_

---

_Comment by @konstin on 2025-06-20 15:35_

This sounds like a problem with the server configuration, https should not need authentication if another protocol doesn't need authentication. Unlike https and ssh, the git transport doesn't have any checks that the data isn't tampered with, so we're reluctant from a security perspective to support an insecure protocol when a secure protocol exists.

---

_Label `enhancement` removed by @konstin on 2025-06-20 15:35_

---

_Label `needs-decision` added by @charliermarsh on 2025-07-11 02:31_

---

_Referenced in [astral-sh/uv#12344](../../astral-sh/uv/issues/12344.md) on 2025-07-11 02:31_

---

_Closed by @charliermarsh on 2025-08-26 02:27_

---

_Reopened by @charliermarsh on 2025-08-26 02:27_

---

_Closed by @charliermarsh on 2025-08-26 02:27_

---
