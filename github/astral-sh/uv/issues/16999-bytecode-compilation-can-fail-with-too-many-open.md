---
number: 16999
title: "Bytecode compilation can fail with \"too many open files\" on default Ubuntu settings"
type: issue
state: open
author: vient
labels:
  - bug
assignees: []
created_at: 2025-12-05T13:22:53Z
updated_at: 2025-12-11T11:10:44Z
url: https://github.com/astral-sh/uv/issues/16999
synced_at: 2026-01-07T13:12:19-06:00
---

# Bytecode compilation can fail with "too many open files" on default Ubuntu settings

---

_Issue opened by @vient on 2025-12-05 13:22_

### Summary

I tried to do `sudo python3 -m uv pip install --compile <a ton of packages>` and got
```
Prepared 688 packages in 51.95s
Uninstalled 13 packages in 51ms
Installed 688 packages in 571ms
error: Failed to bytecode-compile Python file in: /usr/twix/python3.12/lib/python3.12/site-packages
  Caused by: Failed to start Python interpreter to run compile script
  Caused by: Too many open files (os error 24)
```
Open files limit at this time was 1024. `ulimit -n 65536` fixed the issue but it would be nice for uv to check the limit, maybe try to increase it by itself if hard limit permits so.

### Platform

Ubuntu 20.04

### Version

uv 0.9.15

### Python version

Python 3.12.11

---

_Label `bug` added by @vient on 2025-12-05 13:22_

---

_Comment by @zanieb on 2025-12-05 14:41_

cc @EliteTK / @konstin

---

_Renamed from "uv pip install --compile fails with "too many open files" error on default Ubuntu settings" to "Bytecode compilation can fail with "too many open files" on default Ubuntu settings" by @zanieb on 2025-12-05 14:41_

---

_Comment by @samypr100 on 2025-12-06 03:56_

I think this kind of error is pretty common in a lot of software, especially when limits are set very low.

Here's an easy docker repro: `docker run -it --rm --cpus=2 --ulimit nofile=30:30 ghcr.io/astral-sh/uv:python3.13-trixie bash -c 'uv venv; uv pip install --compile jupyter'`

```
Using CPython 3.13.9 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
⠼ pyzmq==27.1.0                                                                                                         error: Failed to write to the client cache
  Caused by: No file descriptors available (os error 24) at path "/root/.cache/uv/simple-v18/pypi/.tmp8TDa6v"
```

There's also other errors that may occur elsewhere, which can be likely due to `--compile` making the situation a tad worse.

```
Using CPython 3.13.9 interpreter at: /usr/local/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 97 packages in 756ms
  x Failed to download `tornado==6.5.2`
  |-> Failed to extract archive:
  |   tornado-6.5.2-cp39-abi3-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl
  |-> I/O operation failed during extraction
  `-> failed to create file `/root/.cache/uv/.tmppREXNl/tornado/__init__.py`: No file descriptors available (os error
      24)
  help: `tornado` (v6.5.2) was included because `jupyter` (v1.1.1) depends on `jupyterlab` (v4.5.0) which depends
        on `tornado`
```

---

_Comment by @konstin on 2025-12-09 10:05_

How many cores does your machine have? Asking for the default uv parallelism settings.

---

_Comment by @vient on 2025-12-09 10:13_

This was observed on machine with 128 cores. Usually I use 32 cores where I did not see this issue.

---

_Comment by @konstin on 2025-12-09 14:37_

That explains it, that means we're failing we have have more than 8 file descriptors open per thread, which includes pipes we need to communicate with the compilation subprocess.

We can adjust the concurrency limit based on the ulimit, and emit a warning if there's a ulimit that's very low compared to the number of cores.

---

_Comment by @konstin on 2025-12-11 11:10_

Another option is to increase the ulimit for our current process below the hard limit using `setrlimit`. The underlying problem is that Linux has an old hardcoded of 1024 open files for a process, which isn't reasonable on a modern 128 core machine - if you do a simple one thread per core model, you get 1028 / 128 = 8 file handles per core!

Ideally, we'd increase this limit permanently, but that requires either root, which many user's don't have, or workarounds such as calling `ulimit` in the user's shell profile, which then only works for subprocess of that shell, but either way `ulimit` is something so low level that I'd rather not expose our users too it only because they have a powerful machine.

There's precedent for increasing the ulimit in the process:
* bun: https://github.com/oven-sh/bun/blob/1d50af7fe86065fbdd72667fed7b80b070ba24bb/src/fs.zig#L785-L818
* nginx: https://github.com/nginx/nginx/blob/c70457482c4223b6fd9adc3caa6a302163e6030d/src/os/unix/ngx_process_cycle.c#L777-L797
* redis: https://github.com/redis/redis/blob/9b7254c8107cee2251c3972c645bf21f23848865/src/server.c#L2576-L2597
* OpenJDK: https://github.com/openjdk/jdk/blob/aa986be7529b7a2950202dbe6885e5224d331078/src/hotspot/os/linux/os_linux.cpp#L4564-L4578

---
