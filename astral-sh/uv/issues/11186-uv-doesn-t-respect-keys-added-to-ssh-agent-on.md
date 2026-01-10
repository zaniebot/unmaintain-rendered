---
number: 11186
title: "uv doesn't respect keys added to ssh-agent on windows"
type: issue
state: closed
author: Mithrandir2k18
labels:
  - question
assignees: []
created_at: 2025-02-03T17:08:56Z
updated_at: 2025-02-10T14:05:36Z
url: https://github.com/astral-sh/uv/issues/11186
synced_at: 2026-01-10T01:25:02Z
---

# uv doesn't respect keys added to ssh-agent on windows

---

_Issue opened by @Mithrandir2k18 on 2025-02-03 17:08_

### Summary

I have some users of a private library that is added as a dependency via git+ssh. This works flawlessly on my linux pc, but if they run the same repo using the same `uv run` command, uv will fail to grab the ssh-key, even if `ssh-add` was called again right before, leading to an error when resolving the dependency.

There doesn't seem to be much documentation about the topic. Does this mean this is a git-config issue? Or is there a way to tell uv which
key to use/load/request? Can we tell uv to respect a given ssh-config file?

### Platform

Windows 10 x86_64

### Version

uv 0.5.26

### Python version

python 3.12.6

---

_Label `bug` added by @Mithrandir2k18 on 2025-02-03 17:08_

---

_Comment by @zanieb on 2025-02-03 18:06_

We just invoke the `git` CLI — it sounds like you're missing some configuration there?

---

_Label `bug` removed by @zanieb on 2025-02-03 18:06_

---

_Label `question` added by @zanieb on 2025-02-03 18:06_

---

_Comment by @Mithrandir2k18 on 2025-02-03 18:18_

Thanks for the fast clarification. I'll check it out on my end and report back.

---

_Comment by @Mithrandir2k18 on 2025-02-10 14:05_

Bug confirmed to be a misconfiguration of the ssh-agent not properly forwarding keys to a devcontainer, causing the git-related error while downloading dependencies. Sorry for the inconvenience.

---

_Closed by @Mithrandir2k18 on 2025-02-10 14:05_

---
