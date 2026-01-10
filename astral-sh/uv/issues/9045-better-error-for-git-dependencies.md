---
number: 9045
title: Better error for git dependencies
type: issue
state: open
author: dimaqq
labels:
  - error messages
assignees: []
created_at: 2024-11-12T04:29:58Z
updated_at: 2024-11-12T14:56:00Z
url: https://github.com/astral-sh/uv/issues/9045
synced_at: 2026-01-10T01:24:36Z
---

# Better error for git dependencies

---

_Issue opened by @dimaqq on 2024-11-12 04:29_

Out of the box:

```command
> uvx --with 'git+https://github.com/tox-dev/tox.git@MISTAKE#egg=tox' tox --version
Updating https://github.com/tox-dev/tox.git (MISTAKE)                                                                                                                          × Invalid `--with` requirement
  ╰─▶ Git operation failed
```

while it points to git, the git dep syntax is a bit quirky and I wish a little more info could be provided...

as a regular end user, I'm left guessing whether I got `git+https` wrong, or org name or repo name or branch name or egg name or perhaps there's a network or credential issue.

uv-tool-uvx 0.5.1 (Homebrew 2024-11-08)


---

_Comment by @dimaqq on 2024-11-12 04:31_

Ok, perhaps "MISTAKE" kinda gives it away, I had a typo in a feature branch name, not main or mistake.

---

_Label `error messages` added by @charliermarsh on 2024-11-12 14:56_

---
