---
number: 14589
title: "thread 'main' panicked at /home/runner/work/uv/uv/crates/uv/src/lib.rs:2276:10:"
type: issue
state: closed
author: sdebruyn
labels:
  - bug
assignees: []
created_at: 2025-07-13T18:29:43Z
updated_at: 2025-11-18T10:44:04Z
url: https://github.com/astral-sh/uv/issues/14589
synced_at: 2026-01-07T13:12:18-06:00
---

# thread 'main' panicked at /home/runner/work/uv/uv/crates/uv/src/lib.rs:2276:10:

---

_Issue opened by @sdebruyn on 2025-07-13 18:29_

### Summary

I have a cronjob running `uv run python ...` in a venv every hour and I always get the following in my stderr:

```
thread 'main2' panicked at /root/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/tokio-1.45.1/src/process/unix/pidfd_reaper.rs:130:14:
pidfd is ready to read, the process should have exited
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

thread 'main' panicked at /home/runner/work/uv/uv/crates/uv/src/lib.rs:2276:10:
Tokio executor failed, was there a panic?: Any { .. }
```

### Platform

Linux frida 6.6.44-production+truenas #1 SMP PREEMPT_DYNAMIC Mon May  5 14:30:30 UTC 2025 x86_64 GNU/Linux

### Version

uv 0.7.20

### Python version

Python 3.12.11

---

_Label `bug` added by @sdebruyn on 2025-07-13 18:29_

---

_Referenced in [tokio-rs/tokio#7144](../../tokio-rs/tokio/issues/7144.md) on 2025-07-13 20:32_

---

_Comment by @zanieb on 2025-07-14 14:10_

Thanks for the report. That's weird. We're surfacing that error from tokio, thanks for linking that issue. Do you think you can get us a minimal reproduction?

---

_Comment by @sdebruyn on 2025-07-14 17:08_

My Python script is called like this: `/mnt/hdds/homes/sam/.local/bin/uv run --directory /mnt/hdds/homes/sam/concerts-notifier python concert_notifier.py`

That directory only contains a Python script named `concert_notifier.py` and the `requirements.txt` file.

This is what is in my `requirements.txt`:
```
openai
spotipy
nltk
requests
```

I can share you a gist of the script privately if you send me a contact detail :)

---

_Comment by @zanieb on 2025-07-14 17:33_

Does the error occur outside of cron?

---

_Comment by @sdebruyn on 2025-07-14 17:46_

It does not seem so.

---

_Comment by @geofft on 2025-07-27 02:02_

Can you log `[i for i in open("/proc/self/status") if i.startswith("Tracer")]` (or print that out in a separate cronjob)? I'm curious if your cron daemon is putting a ptracer on your program.

Is this directly inside cron or inside something like a container?

I have a couple of other random theories (e.g., is this accessing a slow filesystem in threads?) but the failure inside vs. outside cron is a little weird.

---

_Comment by @sdebruyn on 2025-07-28 10:09_

I'm not sure I'm following. Would you like me to just create a Python script with this line? In the meantime I moved to another solution and I'm no longer using a Python script in cron on that machine.

---

_Comment by @cangtianhuang on 2025-11-17 02:42_

Consider reinstalling Python:
```bash
rm -r "$(uv python dir)"
uv python install
```

---

_Comment by @konstin on 2025-11-18 10:44_

This has been fixed in https://github.com/tokio-rs/tokio/pull/7494

---

_Closed by @konstin on 2025-11-18 10:44_

---
