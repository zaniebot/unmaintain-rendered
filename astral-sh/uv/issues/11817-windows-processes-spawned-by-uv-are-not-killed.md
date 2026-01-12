```yaml
number: 11817
title: "[Windows] Processes spawned by uv are not killed when uv is killed"
type: issue
state: open
author: gau-nernst
labels:
  - bug
assignees: []
created_at: 2025-02-27T02:29:58Z
updated_at: 2025-03-01T17:50:12Z
url: https://github.com/astral-sh/uv/issues/11817
synced_at: 2026-01-12T16:00:46Z
```

# [Windows] Processes spawned by uv are not killed when uv is killed

---

_@gau-nernst_

### Summary

```python
# sleep.py
import time

time.sleep(100000)
```

```
uv run sleep.py
```

Then I manually kill uv process in Task manager. I can still see the Python process.

On MacOS/Linux, if I kill it with `kill pid` (which sends SIGTERM), the Python process will exit. Though I also notice that if I kill with SIGKILL instead `kill -9 pid`, the Python process does not stop.

In a larger context, in my application, I'm trying to spawn uv as a subprocess to launch Python apps. And I want to be able to kill the uv subprocess and its python process. Having troubles to do it on Windows right now.

### Platform

Windows 11 64bit

### Version

uv 0.6.2 (6d3614eec 2025-02-19)

### Python version

_No response_

---

_Label `bug` added by @gau-nernst on 2025-02-27 02:29_

---

_Assigned to @zanieb by @zanieb on 2025-02-27 18:49_

---

_Comment by @zanieb on 2025-02-27 18:50_

Thanks for the report. I can look into our implementation of this, might need @Gankra to test Windows-specific parts though.

---

_Comment by @zanieb on 2025-02-27 18:53_

As a note on `kill -9` on Unix — we can't do anything like forward signals there. We'd need to rely on the OS to send SIGKILL to any children too. Are you sending the signal to the _process group id_?

---

_Comment by @zanieb on 2025-02-27 18:55_

On Windows, you probably want `taskkill /t`? https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill

---

_Comment by @gau-nernst on 2025-02-28 00:29_

> As a note on kill -9 on Unix — we can't do anything like forward signals there. We'd need to rely on the OS to send SIGKILL to any children too. Are you sending the signal to the process group id?

I'm just putting my observation there regarding `SIGKILL`. It's not a problem for me. I'm currently using `SIGTERM` (which propagates correctly) on Unix, so that works fine.

> On Windows, you probably want taskkill /t?

Just tried and it worked `taskkill /pid <pid> /t /F`! Currently I'm using Job Object https://learn.microsoft.com/en-us/windows/win32/procthread/job-objects, which seems to serve my purpose well. Since I'm managing this programmatically via C++ code, I will prefer not to spawn another `taskkill` process :). Though I will probably look into how `taskkill` list a process's children.

Perhaps you might want to use Job Object when uv spawns Python process on Windows too? Not sure if there is perf complications.

---

_Comment by @samypr100 on 2025-02-28 02:14_

I think this is expected behavior based on https://github.com/rust-lang/rust/blob/master/library/std/src/sys/pal/windows/process.rs as the child process effectively becomes orphaned when killed that way. I believe killing the process group instead is the right approach I'd take.

---

_Comment by @zanieb on 2025-03-01 17:50_

> Perhaps you might want to use Job Object when uv spawns Python process on Windows too? Not sure if there is perf complications.

That's pretty interesting, there could be something there but I don't foresee having time to pursue it myself right now.

---
