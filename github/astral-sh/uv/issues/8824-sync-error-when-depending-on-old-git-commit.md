---
number: 8824
title: Sync error when depending on old Git commit
type: issue
state: open
author: disketten
labels: []
assignees: []
created_at: 2024-11-05T08:00:55Z
updated_at: 2024-11-17T15:26:57Z
url: https://github.com/astral-sh/uv/issues/8824
synced_at: 2026-01-07T13:12:18-06:00
---

# Sync error when depending on old Git commit

---

_Issue opened by @disketten on 2024-11-05 08:00_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
-->

First, thank you for an amazing program!

I have just ran into an issue when installing/syncing an "old" project. My main project depends on an internal project, like this:

### pyproject.toml
```
...
dependencies = [
  "dependency @ git+ssh://host/path/to/dependency.git"
]
...
```

### uv.lock
```
...
[[package]]
name = "dependency"
version = "1.7.0"
source = { git = "ssh://host/path/to/dependency.git#53f731b8c86f2a1b98071e0f52652b7c8a293d7b" }
dependencies = [
    { name = "hidapi" },
]
...
```

In the dependency project, I do not use tags or branches to distinguish versions, but only the Git commit ID in the main project's uv.lock file. This causes problems in uv when the dependency project has "moved on" (gotten new commits after the locked one):

```shell
$ uv sync
Updating ssh://host/path/to/dependency.git (HEAD)
error: Failed to download and build: `project @ git+ssh://host/path/to/dependency.git`
  Caused by: Git operation failed
  Caused by: failed to fetch into: /home/prod/.cache/uv/git-v0/db/e947297af3188230
  Caused by: failed to fetch commit `53f731b8c86f2a1b98071e0f52652b7c8a293d7b`
  Caused by: process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'ssh://host/path/to/dependency.git' '+53f731b8c86f2a1b98071e0f52652b7c8a293d7b:refs/remotes/origin/HEAD'` (exit status: 1)
--- stderr
error: Server does not allow request for unadvertised object 5
```

Before switching to uv, I used PDM. I guess uv does the "fetch/clone" different, since this use case works in PDM.

# Info
```shell
$ uname -a
Linux hostname 6.8.0-48-generic #48-Ubuntu SMP PREEMPT_DYNAMIC Fri Sep 27 14:04:52 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux  # <-- Ubuntu 24.04.1 LTS

$ uv version
uv 0.4.29
```

---

_Comment by @my1e5 on 2024-11-05 11:41_

I ran into a similar thing. In my case, I host a Gitea server on my local network. I solved it by modifying the `.gitconfig` of the git server. 
```
uploadpack.allowTipSHA1InWant = true
uploadpack.allowReachableSHA1InWant = true
```
Reference - https://github.com/go-gitea/gitea/issues/11958

---
