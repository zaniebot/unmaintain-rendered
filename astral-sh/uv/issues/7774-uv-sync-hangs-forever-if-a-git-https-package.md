```yaml
number: 7774
title: "`uv sync` hangs forever if a git+https package requires authentication"
type: issue
state: closed
author: command-tab
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-09-29T01:51:57Z
updated_at: 2024-09-29T16:49:45Z
url: https://github.com/astral-sh/uv/issues/7774
synced_at: 2026-01-12T15:59:16Z
```

# `uv sync` hangs forever if a git+https package requires authentication

---

_@command-tab_

**Problem**

When running `uv sync` in a project containing a `pyproject.toml` that lists a package via git+https which requires authentication, the uv operation spins forever at "Resolving dependencies...".

When running with `uv --verbose sync`, I see an authentication prompt in the verbose output. It would be helpful if that prompt/non-OK HTTP status code was detected and either prompted about or at least errored about during a non-verbose `uv sync` rather than appearing to indicate that dependencies might resolve if you wait long enough.

**Workaround**

For a quick workaround, I expanded the read permissions of that git repo, since I control it as well.

**Example**

`pyproject.toml`
```toml
[project]
name = "myapp"
version = "1.0.0"
description = "My app"
readme = "README.md"
requires-python = ">=3.11"
# Awaiting resolution on this GitHub issue to be able to use a private pypi
# without specifying an extra index URL, which could be abused:
# https://github.com/astral-sh/uv/issues/171
dependencies = [
  "mypackagename @ git+https://git.example.com/mypackagename.git@1.2.3",
]
```

**Environment**

* macOS 15.0 Sequoia
* Python 3.11.3 (via uv)
* uv 0.4.15 (Homebrew 2024-09-21)


---

_Comment by @command-tab on 2024-09-29 01:56_

(Reported drive-by comment bot to GitHub for abuse)

---

_Comment by @charliermarsh on 2024-09-29 02:00_

(Thank you, blocked that user.)

---

_Comment by @charliermarsh on 2024-09-29 02:10_

I think this may be the same as https://github.com/astral-sh/uv/issues/3783 (albeit with HTTPS -- but same underlying issue of not showing the prompt). Does that seem correct to you?

---

_Label `bug` added by @charliermarsh on 2024-09-29 02:10_

---

_Label `duplicate` added by @charliermarsh on 2024-09-29 02:10_

---

_Comment by @command-tab on 2024-09-29 02:13_

Oh, yup! Sounds like the same behavior, just a different protocol. Feel free to close this as a dupe if you wish to consolidate :) Thank you for uv!!

---

_Closed by @charliermarsh on 2024-09-29 11:51_

---

_Comment by @charliermarsh on 2024-09-29 11:51_

Thanks for following up :)

---
