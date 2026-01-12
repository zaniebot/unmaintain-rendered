```yaml
number: 6002
title: "`uv pip install \"NAME @ git+https://URL\"` prints extraneous warning"
type: issue
state: closed
author: gotmax23
labels:
  - question
assignees: []
created_at: 2024-08-11T05:47:29Z
updated_at: 2024-08-11T20:16:11Z
url: https://github.com/astral-sh/uv/issues/6002
synced_at: 2026-01-12T15:59:00Z
```

# `uv pip install "NAME @ git+https://URL"` prints extraneous warning

---

_@gotmax23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


``` console
$ cat /etc/redhat-release 
Fedora release 40 (Forty)
$ python --version                        
Python 3.12.4
$ uv --version
uv 0.2.35
$ uv pip install 'fedrq @ git+https://git.sr.ht/~gotmax23/fedrq@main'
 Updated https://git.sr.ht/~gotmax23/fedrq (951199b)
Resolved 11 packages in 541ms
░░░░░░░░░░░░░░░░░░░░ [0/11] Installing wheels...
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance. If this is intentional, use `--link-mode=copy` to suppress this warning.

hint: If the cache and target directories are on different filesystems, hardlinking may not be supported.
Installed 11 packages in 10ms
[result truncated]
```

uv should understand that I am attempting to install from a git repository and not print errors about caching. The command still succeeds and installs the package, but uv should accept this standard dependency specification without warnings.

---

_Comment by @zanieb on 2024-08-11 15:08_

I don't think this warning has anything to do with the use of a Git repository. We clone the Git repository into the cache and unzip wheels wheels into the cache, then attempt to link them into the environment. In this case, the warning looks correct in notifying you that your cache is not co-located with your environment and we must fallback to a slow copy operation.

---

_Label `question` added by @zanieb on 2024-08-11 15:08_

---

_Comment by @gotmax23 on 2024-08-11 16:46_

I have been using uv on this system for months, and this is the only time I've seen this error.

---

_Comment by @charliermarsh on 2024-08-11 18:51_

We added it somewhat recently. What's the location of `uv cache dir`, and of the directory you're working in?

---

_Comment by @colinlaney on 2024-08-11 20:05_

The hint in this warning makes a lot of sense. The basic source of the warning is that the target directory is mounted from somewhere (NFS or Docker volume) other than the cache directory or vice versa.
It is a good point that the user is notified on how to improve the performance of the installation process.

---

_Comment by @gotmax23 on 2024-08-11 20:16_

`uv cache dir` and the venv directory are on the same filesystem. I have no idea why this issue popped up, but I can no longer reproduce it, so I guess I'll close this ticket.

---

_Closed by @gotmax23 on 2024-08-11 20:16_

---
