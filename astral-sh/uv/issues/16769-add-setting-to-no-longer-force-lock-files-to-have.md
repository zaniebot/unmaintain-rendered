```yaml
number: 16769
title: Add Setting To No Longer Force Lock Files to have 777 Permissions in Linux
type: issue
state: open
author: delvecccims
labels:
  - enhancement
assignees: []
created_at: 2025-11-18T16:56:03Z
updated_at: 2026-01-12T15:22:47Z
url: https://github.com/astral-sh/uv/issues/16769
synced_at: 2026-01-12T16:02:38Z
```

# Add Setting To No Longer Force Lock Files to have 777 Permissions in Linux

---

_@delvecccims_

### Summary

During a scan of our Linux servers, there were some lock files that were flagged for having World Writable permissions in the temporary directory. It looks like this was changed in February as part of https://github.com/astral-sh/uv/pull/11328. 

There was a question about the security concerns from this change in https://github.com/astral-sh/uv/issues/12665, but there hasn't been a comment yet regarding it. Would it be possible to have a setting that can be enabled to ignore this change so that umask can set the permissions appropriately so that these lock files are no longer flagged in our scans for having World Writable permissions?

### Example

_No response_

---

_Label `enhancement` added by @delvecccims on 2025-11-18 16:56_

---

_Comment by @konstin on 2025-11-18 17:31_

This sounds like a false positive in the security scanner. We use files to coordinate between different processes, which often run with different permissions, e.g. from different containers that mount the same volume. These files are empty and uv doesn't read from them.

---

_Comment by @skikd636 on 2025-11-25 13:39_

This is a security issue and in my book a security violation.

A text file like a lock file should NEVER have 777 permissions.  The most permissions a lock file should have would be 644.  Why would you want anyone to be able to change a lock file or worse execute it?

---

_Comment by @konstin on 2025-11-26 15:48_

See https://github.com/astral-sh/uv/pull/16845 for taking away the executable bit, and thoughts on 666 vs 644 permissions.

---

_Comment by @dcwatson on 2025-12-01 19:41_

My understanding is that locking a file (via `flock`, which Rust's `try_lock` and friends [currently use](https://doc.rust-lang.org/stable/std/fs/struct.File.html#platform-specific-behavior-3)) doesn't require write permissions (beyond being able to create the file if it doesn't exist, which shouldn't be a problem for temp files). From https://linux.die.net/man/2/flock:

> A shared or exclusive lock can be placed on a file regardless of the mode in which the file was opened.

So 644 should still allow anyone to acquire exclusive locks. Does `uv` ever clean these manually, or does it leave that to the OS?

---

_Comment by @zanieb on 2025-12-01 20:19_

Thanks for the reference!

We don't remove lock files manually, no (some previous discussion in https://github.com/astral-sh/uv/issues/10773). We do remove temporary directories we create on drop though.

I'm down to use 644 if someone verifies it doesn't regress.

---

_Comment by @dcwatson on 2025-12-02 13:45_

Using the Dockerfile from the original issue (#11324), slightly modified to show debug logs, the uid, and contents of /tmp:

```Dockerfile
FROM python:3.13
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

WORKDIR /src

RUN uv init && \
    uv add pytest && \
    chmod 644 /tmp/uv-*.lock && \
    useradd --user-group --create-home --uid 1000 team

USER team

RUN id -u && \
    uv run -v pytest -h && \
    ls -la /tmp
```

Running `docker build --no-cache --progress=plain .`, there is a warning about not being able to open the file:

```
#10 0.084 WARN Failed to acquire project environment lock: failed to open file `/tmp/uv-a307630218d6a300.lock`: Permission denied (os error 13)
```

But I suspect that this is because it's trying to open the file as writable, not an error with acquiring a lock?

https://github.com/astral-sh/uv/blob/main/crates/uv-fs/src/lib.rs#L822

It's also tricky to "test" because this is just a warning now, uv doesn't fail if it can't get the lock. I'm not super comfortable with Rust, but I'm happy to test changes or take a stab at a PR. Seems like if uv doesn't need to write anything to the lock file, it shouldn't try to open it as writeable. That plus a 644 mode seems like it would be ideal.

---

_Comment by @ruttenb on 2026-01-12 14:23_

Hey are there any updates with this issue?  Due to the security concern, this is blocking our use of this package.  Thanks!

---

_Comment by @konstin on 2026-01-12 14:24_

We did change the permission: https://github.com/astral-sh/uv/pull/16845. Are you on the latest uv version?

---

_Comment by @ruttenb on 2026-01-12 14:41_

I believe the most recent change updated the permissions to 666, but giving the world write permissions to these lock files is still a concern.  Above there were conversations about updating this to 644 which would resolve the security concern.

---

_Comment by @zanieb on 2026-01-12 15:05_

What's your concrete security concern from this file being writable? What is the attack vector?

Regardless, we can continue investigating changing to 644. The change needs to be validated to ensure it does not cause regressions.

---

_Comment by @ruttenb on 2026-01-12 15:17_

CIS 6.1.10 - "Ensure no world writable files exist".  I attached a PDF of the CIS benchmarks if you scroll down to 6.1.10 you'll see it there.  Here are a couple other links that talk about world write permissions:

- https://www.tenable.com/audits/items/CIS_Red_Hat_EL7_STIG_v2.0.0_STIG.audit:9d80f2f418addc905a8d399be44485c0
- https://docs.datadoghq.com/security/default_rules/def-000-dv8/

[CIS_Oracle_Linux_6_Benchmark_v100.pdf](https://github.com/user-attachments/files/24567632/CIS_Oracle_Linux_6_Benchmark_v100.pdf)

---

_Comment by @zanieb on 2026-01-12 15:22_

Sorry but those are just best practice security rules, there's nothing there that talks about concrete attack vectors.

---
