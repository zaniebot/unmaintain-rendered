```yaml
number: 16079
title: Missing libssh.h headers cause silent hang
type: issue
state: closed
author: nedbat
labels:
  - external
assignees: []
created_at: 2025-09-30T19:56:56Z
updated_at: 2025-10-24T12:45:46Z
url: https://github.com/astral-sh/uv/issues/16079
synced_at: 2026-01-10T03:23:54Z
```

# Missing libssh.h headers cause silent hang

---

_Issue opened by @nedbat on 2025-09-30 19:56_

### Summary

On my Mac (15.7.1), I run:
```
% uv pip install ansible-pylibssh
Resolved 1 package in 328ms
   Building ansible-pylibssh==1.2.2
⠇ Preparing packages... (0/1)
```
and it stops, never finishing.

Adding `-v`, I see:

```
DEBUG Tried 5 versions: cython 1, expandvars 1, packaging 1, setuptools 1, setuptools-scm 1
DEBUG marker environment resolution took 0.039s
DEBUG Installing in packaging==25.0, expandvars==1.1.2, setuptools-scm==9.2.0, cython==3.1.4, setuptools==80.9.0 in /Users/nbatchelder/.cache/uv/builds-v0/.tmpjdiNy3
DEBUG Requirement already installed: packaging==25.0
DEBUG Requirement already installed: expandvars==1.1.2
DEBUG Requirement already installed: setuptools-scm==9.2.0
DEBUG Registry requirement already cached: cython==3.1.4
DEBUG Requirement already installed: setuptools==80.9.0
DEBUG Installing build requirement: cython==3.1.4
DEBUG Calling `pep517_backend.hooks.build_wheel("/Users/nbatchelder/.cache/uv/builds-v0/.tmpGJIDGB", {}, None)`
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/_libssh_version.pyx
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/errors.pyx
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/sftp.pyx
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/session.pyx
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/channel.pyx
DEBUG [1/1] Cythonizing /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/scp.pyx
DEBUG /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/sftp.c:1138:10: fatal error: 'libssh/libssh.h' file not found
DEBUG  1138 | #include "libssh/libssh.h"
DEBUG       |          ^~~~~~~~~~~~~~~~~
DEBUG /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/errors.c/private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/channel.c::11371138::1010::  fatal error: fatal error: 'libssh/libssh.h' file not found'libssh/libssh.h' file not found
DEBUG
DEBUG /private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/session.c :11371138 | :#10i:n cfatal error: l'libssh/libssh.h' file not foundu
DEBUG de/private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/scp.c: 1138:10:  fatal error: 1138'libssh/libssh.h' file not found |
DEBUG #/private/var/folders/26/r96mj7hd38s7_2djcsd9qqp80000gn/T/.tmp-ansible-pylibssh-pep517-6g3n_u3e/src/src/pylibsshext/_libssh_version.cin:c1135l:u10" li1138b | s# i1138d | #ei ns:nhc/lluidbes ""lliibbscslhsssh/hu.dhe"
DEBUG "      l fatal error: l'libssh/libssh.h' file not foundi
DEBUG b| s         ^~~~~~~~~~~~~~~~~s
DEBUG h/ /liilbis1135s | h.h"
DEBUG       #inclubbdses "slsh.h"
DEBUG       |          ^~~~~~~~~~~~~~~~~
DEBUG h| .h         ^~~~~~~~~~~~~~~~~
DEBUG "
DEBUG       |          ^~~~~~~~~~~~~~~~~
DEBUG ibssh/libssh.h"
DEBUG       |          ^~~~~~~~~~~~~~~~~
DEBUG 1 error generated.
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
DEBUG 1 error generated.
DEBUG 1 error generated.
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
DEBUG 1 error generated.
DEBUG 1 error generated.
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
DEBUG 1 error generated.
DEBUG error: command '/usr/bin/gcc' failed with exit code 1
```
(sometimes the errors are interleaved as here, sometimes not.)

### Platform

Mac 15.7.1

### Version

uv 0.8.22 (ade2bdbd2 2025-09-23)

### Python version

Python 3.11.13

---

_Label `bug` added by @nedbat on 2025-09-30 19:56_

---

_Comment by @zanieb on 2025-09-30 19:59_

This is a `ansible-pylibssh` build problem. I'm not sure why they don't exit on failure. I don't think we can time out builds, since they sometimes do legitimately take a long time. I think you'll need to take this up with `cython` and / or `ansible-pylibssh`.

---

_Label `external` added by @zanieb on 2025-09-30 19:59_

---

_Label `bug` removed by @konstin on 2025-10-01 09:54_

---

_Comment by @konstin on 2025-10-01 09:54_

I can reproduce the same hang with `python -m build` after cloning their repository.

---

_Comment by @nedbat on 2025-10-01 11:26_

OK, thanks.  Maybe uv could  try to not interleave the error messages, but I guess that's a separate issue.

---

_Closed by @nedbat on 2025-10-01 11:26_

---

_Comment by @webknjaz on 2025-10-22 02:05_

The upstream fix is in (https://github.com/cython/cython/pull/7183 / https://github.com/cython/cython/commit/440b19a36e3c145f6ba6a7b84199c246c1902184). It is supposed to become a part of Cython 3.2b1.

---

_Comment by @webknjaz on 2025-10-24 12:45_

*UPD:* it was also backported into Cython 3.1.6.

---
