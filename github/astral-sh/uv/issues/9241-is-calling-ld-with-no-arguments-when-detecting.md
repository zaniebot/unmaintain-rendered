---
number: 9241
title: "Is calling `ld` with no arguments when detecting Linux environment intended?"
type: issue
state: closed
author: notatallshaw-gts
labels:
  - question
assignees: []
created_at: 2024-11-19T20:00:05Z
updated_at: 2024-11-19T20:04:47Z
url: https://github.com/astral-sh/uv/issues/9241
synced_at: 2026-01-07T13:12:18-06:00
---

# Is calling `ld` with no arguments when detecting Linux environment intended?

---

_Issue opened by @notatallshaw-gts on 2024-11-19 20:00_

This is purely observational, there is no known bug this is creating.

But I'm looking at adopting uv for bootstrapping the Python executable into our environment, and I happen to be running with trace logs and  I noticed the following output on running `uv venv --python 3.11.10`:

```
#11 0.409 TRACE ld path: /lib64/ld-linux-x86-64.so.2
#11 0.410 TRACE stdout output from `ld`: ""
#11 0.410 TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]\nYou have invoked `ld.so', the helper program for shared library executables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared libraries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a dynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n"
#11 0.410 TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
```

It looks like `ld` was perhaps called without any arguments and it's giving the help docs back, is this intended?

---

_Comment by @notatallshaw-gts on 2024-11-19 20:01_

I should note this is running inside docker and getting uv by running:

```
COPY --from=ghcr.io/astral-sh/uv:0.5.3 /uv /uvx /bin/
```

---

_Comment by @zanieb on 2024-11-19 20:03_

I think we are explicitly trying to parse the help documentation, yeah.

https://github.com/astral-sh/uv/blob/d08bfee718372fc2a271a87e4c5b673874068554/crates/uv-python/src/libc.rs#L138-L146

---

_Label `question` added by @zanieb on 2024-11-19 20:03_

---

_Comment by @notatallshaw-gts on 2024-11-19 20:04_

Thanks, makes sense (in the sort of hacky way that's required sometimes).

---

_Closed by @notatallshaw-gts on 2024-11-19 20:04_

---
