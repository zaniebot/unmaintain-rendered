---
number: 2767
title: Windows locking crash
type: issue
state: closed
author: charlesnicholson
labels:
  - bug
  - windows
assignees: []
created_at: 2024-04-02T00:35:23Z
updated_at: 2024-04-03T14:02:42Z
url: https://github.com/astral-sh/uv/issues/2767
synced_at: 2026-01-07T13:12:17-06:00
---

# Windows locking crash

---

_Issue opened by @charlesnicholson on 2024-04-02 00:35_

I'm running uv v0.1.27 on GitHub Actions on macOS x64, Linux x64, and Windows x64 runners. All jobs in my matrix succeed except for the Windows one, which reliably crashes out with an interesting OS error about a file being partially locked.

I suspect it comes from the ninja build engine aggressively invoking multiple instances of uv very closely in time; when I turn on the timestamps I see the following:
```
Tue, 02 Apr 2024 00:11:28 GMT [33/298] UV //fi.package:fi.package__editable_install(//gntools/toolchains:msvc)
Tue, 02 Apr 2024 00:11:31 GMT FAILED: bin/venv/gn_stamps/fi.package_editable_install.timestamp 
Tue, 02 Apr 2024 00:11:31 GMT C:/hostedtoolcache/windows/Python/3.11.8/x64/python.exe ../gntools/fi_run.py --stamp bin/venv/gn_stamps/fi.package_editable_install.timestamp --env VIRTUAL_ENV=bin/venv -- ../external/vendor/uv/win-x64/uv.exe -q pip install -e ../fi.package -c ../build/venv/constraints.txt
Tue, 02 Apr 2024 00:11:31 GMT error: The process cannot access the file because another process has locked a portion of the file. (os error 33)
Tue, 02 Apr 2024 00:11:31 GMT [34/298] WHEEL //fi.otherpackage:fi.otherpackage__wheel(//gntools/toolchains:msvc)
Tue, 02 Apr 2024 00:11:31 GMT [35/298] UV //fi.otherpackage:fi.otherpackage__editable_install(//gntools/toolchains:msvc)
Tue, 02 Apr 2024 00:11:31 GMT ninja: build stopped: subcommand failed.
```

The `fi_run.py` is just a wrapper that stamps a file on success, and sets optional environment variables in the process it launches (we use `VIRTUAL_ENV` to point uv at the right directory).

It looks like `fi.package` and `fi.otherpackage` are both being scheduled for concurrent execution by ninja. Each one runs `uv pip install -e path/to/package`, and the `uv` instances seem to be fighting for a lock of some kind? (The `WHEEL` command doesn't use `uv` but does use the venv to run `python -m build ...`. I don't suspect it yet...)

When I re-introduce a build lane (pool) of size 1, which forces ninja to only schedule one `uv` instance at a time, Windows succeeds. I've never seen this `os error 33` issue before, so I suspect it's new with `uv` as I introduce it into my codebase, and perhaps has something to do with locking the cache or the venv, depending on what it's doing?

I'm happy to run any experiments with custom branches if it's helpful. Thanks for uv!

---

_Comment by @zanieb on 2024-04-02 02:23_

Interesting thanks for the all the details!

We _should_ be robust to concurrent execution (#2730) but I wouldn't be surprised if there were some edge-cases on Windows in particular.

cc @konstin   

---

_Label `bug` added by @zanieb on 2024-04-02 02:23_

---

_Label `windows` added by @zanieb on 2024-04-02 02:23_

---

_Comment by @konstin on 2024-04-02 10:05_

Is the error from ninja or from uv? For background, we create and lock `bin/venv/.lock` to prevent concurrent modification of the venv, which should queue `fi.package` and `fi.otherpackage` after another. Could it be that in your case uv is too fast for ninja, causing a race where pip was slow enough that this happened sequentially? (we had this kind of error happen before (https://github.com/python/cpython/issues/75953#issuecomment-1846131495))

---

_Comment by @charlesnicholson on 2024-04-02 12:33_

It seems unlikely that the error would lie in ninja, but of course it's possible. 

FWIW ninja is used to build chrome on windows 100s of times per day on Google CI, and chrome has ~50k+ targets, many of which complete in ~1ms. Over the past 10+ years, that ninja-scheduled build hits pretty much every race condition that can exist, at every level of the stack! :)

I'll do a few experiments outside of our build and ninja, maybe I can tease it out in a raw python/C++ example on my Win11 machine.

It may well be my output timestamp code, but the build system errors if any targets write to the same output file. Things are happening very quickly, though, so I'll also look into whether we're explicitly flushing all of our disk io.

---

_Comment by @konstin on 2024-04-02 12:47_

Could you try `konsti/add-filename-to-like-error` (https://github.com/astral-sh/uv/compare/konsti/add-filename-to-like-error) to check if we get more context? It looks like the locking doesn't go through fs_err, that's why it has degraded error messages without filenames (sorry for assuming this was ninja initially, i incorrectly assume we had added filenames / context to all error messages)

---

_Comment by @charlesnicholson on 2024-04-02 13:20_

No apology needed; it wasn't the best bug report: "hey, your tool may or may not be crashing, here are logs from my build system with 3+ layers of indirection before it hits your tool" :)

Anyway, I tried your branch and the logs showed this:
```
error: Could not try lock bin\venv\.lock: The process cannot access the file because another process has locked a portion of the file. (os error 33)
```

So that looks like a clue!

---

_Comment by @konstin on 2024-04-02 13:24_

Oh it's the try part that fails! I'll see about a PR that handles that better tomorrow, we assumed we're getting a `std::io::ErrorKind::WouldBlock` but apparently we aren't.

---

_Comment by @charlesnicholson on 2024-04-02 13:26_

Nice! Feel free to ping me here if you'd like me to try any PR branches. I'm holding steady by just having the dedicated uv build lane only for the Windows build; uv has been a huge speedup over pip even when running 100% serially.

---

_Referenced in [astral-sh/uv#2800](../../astral-sh/uv/pulls/2800.md) on 2024-04-03 08:56_

---

_Comment by @konstin on 2024-04-03 08:57_

Could you try https://github.com/astral-sh/uv/pull/2800? I've reproduced the failure on windows and this fixes it

---

_Comment by @charlesnicholson on 2024-04-03 13:08_

Works great over here, thanks for the attention and very quick fix!

---

_Closed by @konstin on 2024-04-03 14:02_

---
