---
number: 11220
title: uv run without --script forks thousands of processes, then fails, on file without .py extension
type: issue
state: closed
author: ssanderson
labels:
  - bug
assignees: []
created_at: 2025-02-04T17:02:26Z
updated_at: 2025-02-12T18:58:45Z
url: https://github.com/astral-sh/uv/issues/11220
synced_at: 2026-01-07T13:12:18-06:00
---

# uv run without --script forks thousands of processes, then fails, on file without .py extension

---

_Issue opened by @ssanderson on 2025-02-04 17:02_

### Summary

One of the most useful features of `uv`, for me, is the ability to define small [utility scripts](https://docs.astral.sh/uv/guides/scripts/) packaged alongside their dependencies. This allows me to easily use Python for many of the things I would have previously used bash for. This becomes particularly nice when [using uv as your shebang line](https://akrabat.com/using-uv-as-your-shebang-line/).

I recently did this on a small helper script which did not have a `.py` extension and I forgot to include `--script`. The following reproduces the scenario:

```bash
cat <<EOF > ./repro
#!/usr/bin/env -S uv run

# /// script
# dependencies = []
# ///

print("Hello world")
EOF

./repro
```

Running this input causes uv to hang for a while, consuming a large amount of memory (around 8GB), before eventually failing:

```
$ ./repro.sh
error: Failed to spawn: `./repro`
  Caused by: Argument list too long (os error 7)
```

Looking at the processes on my machine during the hang, it appears that uv is spawning hundreds to thousands of subprocesses:

```
$ ps -eaf | grep uv
ssander+  473650  473648  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473652  473650  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473654  473652  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473656  473654  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473658  473656  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473660  473658  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473662  473660  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473664  473662  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473666  473664  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473668  473666  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473670  473668  0 11:55 pts/1    00:00:00 uv run ./repro
ssander+  473672  473670  0 11:55 pts/1    00:00:00 uv run ./repro
// ... many more lines omitted
```

I assume there's some kind of recursion happening here which is effectively causing uv to forkbomb my machine.

The issue described here goes away if I add  `--script` to the shebang line of my script, or if I rename the file to `repro.py`.

---

I don't necessarily expect uv to support the way I've invoked it here. It seems reasonable to me that `uv run` requires the `--script` flag when running a file without a `.py` extension, but it would be nice if it handled this class of user error a bit more gracefully.

### Platform

Ubuntu 24.04, Linux 6.8.0-52-generic x86_64 GNU/Linux

### Version

uv 0.5.27

### Python version

Python 3.12.3

---

_Label `bug` added by @ssanderson on 2025-02-04 17:02_

---

_Comment by @charliermarsh on 2025-02-04 17:05_

Thanks! I believe this is a duplicate of https://github.com/astral-sh/uv/issues/6360.

---

_Comment by @ssanderson on 2025-02-04 17:09_

@charliermarsh yeah, this is the same failure mode. I guess the additional question or feature request here is "is there a way to make uv handle this class of pebkac more gracefully?" As noted above, I think it's reasonable for `uv` to require `--script` in this context, but it also seems like an easy mistake for a user to leave that off, and getting fork-bombed seems like an unfortunate consequence. I realize this might be challenging to solve given the way `uv run` works though.

---

_Comment by @ssanderson on 2025-02-04 17:22_

I think the two ways I could imagine trying to "solve" this would be:

1. Adding some form of dynamic recursion detection to `uv run` to detect if uv is running as a deeply-nested child of itself.
2. Trying to read the target file to see if it's a text file with a shebang line starting with: `/usr/bin/env -S uv run` without `-s` or `--script`.

Both of these feel a bit kludgy to me, but I think both could probably be made to work well enough to catch most reasonable ways that a user might make these mistakes. I'd be happy to take a look at implementing one or the other of these as options if there's interest.

---

_Comment by @charliermarsh on 2025-02-04 17:28_

Yeah I think it makes sense to try and do a bit better here... There have been a few other requests too that would benefit from somehow having access to the command we're running.

---

_Comment by @ssanderson on 2025-02-04 21:17_

@charliermarsh would you be interested in seeing a PR toward either of the potential fixes outlined above? Without having dug too much into the details, I think one concrete proposal toward **(1)** might be something like:

Whenever `uv run` spawns a child process, check if the current process has an environment variable named something like `UV_RUN_RECURSION_DEPTH` set. If not set, spawn the subprocess with `UV_RUN_RECURSION_DEPTH=1`. If set, attempt to parse the current depth as an int, error if greater than some threshold (e.g.,10), and otherwise spawn a subprocess with the recursion depth counter incremented.

I think this would be a fairly robust and low-overhead way to catch the recursion issue here without needing to know more semantic information about the file being executed. But there are certainly other reasonable options.

---

_Comment by @charliermarsh on 2025-02-04 21:23_

Yeah it's similar to https://github.com/astral-sh/uv/issues/8775. The other request I received recently was a way to tell what the current `uv run` command was within the executed process. I'd like to solve that problem too...

@zanieb -- thoughts on `UV_RUN_RECURSION_DEPTH` and `UV_RUN_COMMAND` being propagated to the child process?

---

_Comment by @zanieb on 2025-02-04 21:30_

I'm fine with that. Random thoughts...

- Is there any security risk to propagating `UV_RUN_COMMAND` to children? Could you accidentally leak something?
- We probably want to set `UV=<self-path>` whenever we spawn a child
- Do we want to namespace the recursion tracking variable so it doesn't look user-facing? Like `UV__RUN_DEPTH`?
- Is the recursion limit configurable?

---

_Comment by @charliermarsh on 2025-02-04 21:40_

> Is there any security risk to propagating UV_RUN_COMMAND to children? Could you accidentally leak something?

Possibly... You already have access to `sys.argv` (i.e., any arguments to the script itself), and I believe we propagate all the environment variables. So the only risk would be in command-line arguments, e.g., you could have credentials in an index URL?

---

_Comment by @zanieb on 2025-02-04 21:52_

For `UV_RUN_COMMAND` in particular, I'd like to get a sense for the use-case. I think `UV=<path>` is simple enough to add without controversy.

---

_Comment by @charliermarsh on 2025-02-04 22:32_

`UV__RUN_DEPTH` seems relatively uncontroversial, though determining the correct limit is... unclear. It'd be nice if we could show this as a hint on failure, but the depth could be enormous.

---

_Comment by @zanieb on 2025-02-04 22:54_

I think a default `UV_RUN_RECURSION_LIMIT` of like.. 100 seems reasonable? 1000?

---

_Referenced in [astral-sh/uv#11386](../../astral-sh/uv/pulls/11386.md) on 2025-02-10 14:47_

---

_Comment by @ssanderson on 2025-02-10 15:10_

@zanieb @charliermarsh: PR with an initial crack at the infinite recursion detection here: https://github.com/astral-sh/uv/pull/11386

---

_Closed by @zanieb on 2025-02-12 18:58_

---

_Closed by @zanieb on 2025-02-12 18:58_

---

_Referenced in [astral-sh/uv#11553](../../astral-sh/uv/pulls/11553.md) on 2025-02-19 13:54_

---
