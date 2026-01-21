```yaml
number: 17646
title: Create lock files 644, only check read bit for locks, respect umask
type: pull_request
state: open
author: dcwatson
labels:
  - "test:extended"
assignees: []
base: main
head: 16769-lockfile-perms
created_at: 2026-01-21T20:09:30Z
updated_at: 2026-01-21T20:39:49Z
url: https://github.com/astral-sh/uv/pull/17646
synced_at: 2026-01-21T21:19:24Z
```

# Create lock files 644, only check read bit for locks, respect umask

---

_@dcwatson_

## Summary

Resolves https://github.com/astral-sh/uv/issues/16769 by creating the temp lock files 644, and not requiring write access when trying to acquire a lock on a file. Additionally, I removed the forcing of permissions to override umask -- if a umask is set more restrictively it was probably for a reason, and now that failing to acquire a lock is just a warning, it doesn't seem worth it to override.

## Test Plan

Manually, either via docker or CLI, as it requires separate users to both run a `uv` command that takes a lock on the same project.

I tested with a locally built `uv`. I ran an initial sync using `root`, and verified the lock file was as expected:

```
dcwatson@alektra T % ls -l uv*
.rw-r--r-- 0 root staff 2026-01-21 14:48 uv-ff189de60ce1a681.lock
```

Then running a sync via my own user:

```
DEBUG Acquired exclusive lock for `/Users/dcwatson/testapp`
DEBUG Reading Python requests from version file at `/Users/dcwatson/testapp/.python-version`
DEBUG Using Python request `3.14` from version file at `.python-version`
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.14`
DEBUG Released lock at `/var/folders/qs/c9v6ntlx05x4mt88cgs9p4sc0000gn/T/uv-ff189de60ce1a681.lock`
```

Finally, I modified the lock file so it was owner-read-only (`chmod 400`), and verified a warning (but not an error) was logged when I ran a sync as my own user:

```
WARN Failed to acquire project environment lock: failed to open file `/var/folders/qs/c9v6ntlx05x4mt88cgs9p4sc0000gn/T/uv-ff189de60ce1a681.lock`: Permission denied (os error 13)
```

---

_Assigned to @zanieb by @zanieb on 2026-01-21 20:15_

---

_Comment by @zanieb on 2026-01-21 20:32_

cc @EliteTK since this code last changed in https://github.com/astral-sh/uv/pull/17115

---

_Label `test:extended` added by @zanieb on 2026-01-21 20:39_

---
