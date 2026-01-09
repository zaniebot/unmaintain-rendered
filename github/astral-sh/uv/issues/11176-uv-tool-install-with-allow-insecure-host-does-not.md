---
number: 11176
title: "`uv tool install` with `--allow-insecure-host` does not work as expected"
type: issue
state: closed
author: doraeric
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-02-03T06:46:52Z
updated_at: 2025-02-04T15:57:58Z
url: https://github.com/astral-sh/uv/issues/11176
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv tool install` with `--allow-insecure-host` does not work as expected

---

_Issue opened by @doraeric on 2025-02-03 06:46_

### Summary

I want to install a package from my internal gitlab repo with the following command

```sh
uv tool install --allow-insecure-host git.example.com git+https://git.example.com/user/repo.git
```

but I got some errors

```
Updating https://git.example.com/user/repo.git (HEAD)                                                                                   error: Git operation failed
  Caused by: failed to fetch into: /home/user/.cache/uv/git-v0/db/xxx
  Caused by: process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://git.example.com/user/repo.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
--- stderr
fatal: unable to access 'https://git.example.com/user/repo.git/': server certificate verification failed. CAfile: none CRLfile: none
```

When looking for existing issues, I found this one https://github.com/astral-sh/uv/issues/8555 . Although they don't seem to be related.

I can bypass this issue by setting the environment variable `GIT_SSL_NO_VERIFY=true` though, as instructed [here](https://stackoverflow.com/a/19363404/3566759).

### Platform

Ubuntu 22.04 (Linux 5.15.0-126-generic x86_64 GNU/Linux)

### Version

uv 0.5.18

### Python version

Python 3.12.8

---

_Label `bug` added by @doraeric on 2025-02-03 06:46_

---

_Comment by @zanieb on 2025-02-03 13:52_

Sorry, we invoke the Git CLI directly and it does not use our HTTP client or stack. I guess we could set this for you?

cc @charliermarsh wdyt?

---

_Comment by @zanieb on 2025-02-03 13:53_

I wonder if we can limit the target with `http.<url>.*`, not clear how to use that outside a config file.

---

_Comment by @charliermarsh on 2025-02-03 15:48_

Yeah, I guess we can check if the URL has `--allow-insecure-host` set, and pass the env var in those cases?

---

_Label `help wanted` added by @zanieb on 2025-02-03 16:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-04 02:41_

---

_Comment by @charliermarsh on 2025-02-04 02:56_

Should it be:

```
--trusted-host git+https://localhost:8443
```

or:

```
--trusted-host https://localhost:8443
```

---

_Comment by @zanieb on 2025-02-04 03:06_

I don't have a strong preference. I feel like it's not super relevant that `git` is being used as the protocol. It's about the HTTPS layer.

---

_Referenced in [astral-sh/uv#11210](../../astral-sh/uv/pulls/11210.md) on 2025-02-04 03:19_

---

_Closed by @charliermarsh on 2025-02-04 15:57_

---

_Closed by @charliermarsh on 2025-02-04 15:57_

---
