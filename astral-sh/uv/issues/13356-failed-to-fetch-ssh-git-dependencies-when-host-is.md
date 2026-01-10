---
number: 13356
title: Failed to fetch ssh+git dependencies when host is not known
type: issue
state: open
author: mia-trilo
labels:
  - bug
assignees: []
created_at: 2025-05-08T23:45:37Z
updated_at: 2025-05-09T21:04:06Z
url: https://github.com/astral-sh/uv/issues/13356
synced_at: 2026-01-10T01:25:32Z
---

# Failed to fetch ssh+git dependencies when host is not known

---

_Issue opened by @mia-trilo on 2025-05-08 23:45_

### Summary

`uv sync` seems to fail to install dependencies from an git link with SSH when the host isn't known. Based on the log it seems to be stuck on the "yes/no/fingerprint" prompt when ssh into a new host for the first time. Interestingly, cancelling and re-running `uv sync` again solves this problem, but it can be consistently reproduced with the `--no-cache` flag

To reproduce this issue:

1. Create an empty project with the following `pyproject.toml`
```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["uv"]

[tool.uv.sources]
uv = {git = "ssh://git@github.com/astral-sh/uv.git"}
```
2. Run `ssh-keygen -R github.com`. You can also manually delete all sources of `github.com` in `.ssh/known_hosts`
3. Run `uv sync --no-cache`, observe that this process hangs

### Platform

macOS 15, M3. Also encountered on Raspberry Pi 4B

### Version

0.7.3

### Python version

3.12.4

---

_Label `bug` added by @mia-trilo on 2025-05-08 23:45_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-05-09 08:20_

---

_Comment by @jtfmumm on 2025-05-09 08:40_

Thank your for filing this. Following your reproduction, I get the `yes/no/[fingerprint]` prompt since the host is unknown. If I enter `yes` then things continue as expected. If I run `uv clean` (clearing the cache), then `uv sync` will always hit the prompt after entering `no` and running again.

Just to be clear, when you say uv fails to fetch git dependencies, do you mean that you see the prompt and would need to manually enter `yes` to continue? Or are you seeing something else?

 

---

_Comment by @mia-trilo on 2025-05-09 17:59_

So there are two issues here: first is, without running it in verbose mode I don't even see the prompt (looks like it got erased by the progress animation?), which made the impression that uv hangs without an apparent reason. I have not tried whether I could type yes to have it continue. See below screenshot for what it looks like in my actual repository and how there isn't  a prompt.

<img width="569" alt="Image" src="https://github.com/user-attachments/assets/6b6b4f66-fe4a-43a2-b053-ec93e9ff3f47" />

Second, I encountered this as part of a package installation script. Having to manually intervene makes it more difficult to make the script work (presumably I would have to run `yes | uv sync` for every installation) without knowing the condition on the host machine

---

_Comment by @jtfmumm on 2025-05-09 21:04_

Thanks for clarifying. We’ll investigate this. 

---
