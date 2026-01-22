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
updated_at: 2026-01-21T23:48:31Z
url: https://github.com/astral-sh/uv/pull/17646
synced_at: 2026-01-22T00:09:10Z
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

_Comment by @dcwatson on 2026-01-21 21:04_

FWIW, I just verified the manual test from #17115 still works:

```
$ hdiutil create -size 1g -fs ExFAT -volname EXFATDISK exfat.dmg
$ hdiutil attach exfat.dmg
$ cd /Volumes/EXFATDISK
$ uv init --bare --cache-dir build/uv/cache -v
```

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:316 on 2026-01-21 23:11_

This won't break the testcase on ExFAT because ExFAT doesn't have a concept of permission bits so it won't matter, but the reason this code was here was to cater for whatever filesystem doesn't support renameat2 but does support permission bits so there's no reason to remove it in this PR unless I'm missing something.

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:247 on 2026-01-21 23:15_

If we don't need to write, this should just be `0o444`.

---

_@EliteTK reviewed on 2026-01-21 23:27_

Thanks.

Overall I don't see anything wrong with the idea. Changing the mode to `0o444` will be a good test to ensure that we can still take exclusive locks on MacOS.

I am also curious to see what error message we get if umask is 777, but I have a feeling that we'll never get this far in that case, although you could use fault injection with `strace` to test it.

---

_@EliteTK reviewed on 2026-01-21 23:38_

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:316 on 2026-01-21 23:38_

To clarify, the code shouldn't be removed but should be amended to respect umask.

So... e.g.: `mode != (DESIRED_MODE & !umask)` ... and then have `try_set_permissions` also `&` with `!umask`.

---

_@EliteTK reviewed on 2026-01-21 23:45_

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:285 on 2026-01-21 23:45_

This should also have had `.write(true)` dropped. But instead, it should actually just use `.mode(0o444)`.

---

_@EliteTK reviewed on 2026-01-21 23:46_

---

_Review comment by @EliteTK on `crates/uv-fs/src/locked_file.rs`:316 on 2026-01-21 23:46_

Actually, scratch that.

See the comments above about using `.mode()`, that should be sufficient and then this code isn't necessary. Not sure how I didn't think of this the first time.

---
