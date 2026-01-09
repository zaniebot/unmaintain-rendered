---
number: 13923
title: Pulling wrong files from bitbucket
type: issue
state: closed
author: odellus
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-06-09T16:22:55Z
updated_at: 2025-06-09T16:28:42Z
url: https://github.com/astral-sh/uv/issues/13923
synced_at: 2026-01-07T13:12:18-06:00
---

# Pulling wrong files from bitbucket

---

_Issue opened by @odellus on 2025-06-09 16:22_

### Summary

I recently moved from an old CentOS system where I was able to successfully use a project by having my pyproject.toml file look like this

```toml
[project]
name = "test-example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "repo_name @ git+ssh://git@bitbucket.org/my-development-team/repo_name.git@branch_name",
]

```

Worked like a charm on CentOS 7, but now I am seeing that uv is pulling the wrong files from bitbucket. Completely different files. Not the most recent commit to `branch_name`

Running on this version of Linux
```
Linux version 6.1.134-152.225.amzn2023.x86_64 (mockbuild@ip-10-0-33-51) (gcc (GCC) 11.5.0 20240719 (Red Hat 11.5.0-5), GNU ld version 2.41-50.amzn2023.0.3) #1 SMP PREEMPT_DYNAMIC Wed May  7 09:10:59 UTC 2025
```

### Platform

Amazon Linux

### Version

 6.1.134-152.225.amzn2023.x86_64

### Python version

3.8.20

---

_Label `bug` added by @odellus on 2025-06-09 16:22_

---

_Comment by @zanieb on 2025-06-09 16:27_

Can you please share a reproducible example? e.g., using a public Git repository?

---

_Label `needs-mre` added by @zanieb on 2025-06-09 16:28_

---

_Closed by @odellus on 2025-06-09 16:28_

---

_Comment by @odellus on 2025-06-09 16:28_

I just figured out what was wrong. Didn't have a build file in .gitignore. Thanks for quick reply though! Closing.

---
