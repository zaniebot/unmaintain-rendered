```yaml
number: 16399
title: "`uv run` does not work if `TMPDIR` does not exist"
type: issue
state: open
author: maxvonhippel
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-10-21T21:22:12Z
updated_at: 2025-11-06T04:04:30Z
url: https://github.com/astral-sh/uv/issues/16399
synced_at: 2026-01-12T16:02:30Z
```

# `uv run` does not work if `TMPDIR` does not exist

---

_@maxvonhippel_

### Summary

If `TMPDIR` is set to a dir which does not exist then `uv run foo.py` will fail, regardless of the contents of `foo.py`.

I think the ideal behavior would be either to fallback to `/tmp`, or, to print a message saying `uv run doesn't work if TMPDIR doesn't exist. Currently it's set to ${TMPDIR}`.

I discovered this on a server of mine (I replaced a mount path with `[omitted mount point]` below for privacy):

```
➜  benchify-fixer git:(10-21-ben-2815_make_mount_point_an_env_variable_with_safe_fallbacks) porter app run -e benchify-fixer-staging -- /bin/bash
A new version of the porter CLI is available. Run the following to update: brew install porter-dev/porter/porter
View CLI installation and upgrade docs at https://docs.porter.run/cli/installation

Attempting to run /bin/bash for application benchify-fixer-staging
using remote kubeconfig
Connecting to existing pod which is running an image named: 471112799737.dkr.ecr.us-east-1.amazonaws.com/benchify-fixer:4c61c9a253b2d128042f4dc86c391ec32b838fde
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app#
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app#
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app#
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app# cat foo.py
print('banana')
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app# uv run foo.py
error: No such file or directory (os error 2) at path "/[omitted mount point]/tmp/.tmpgesvWc"
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app#
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app#
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app# TMPDIR=/tmp uv run foo.py
banana
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app# ls /tmp
scarf-js-history-root.log  uv-1c83b73deef05048.lock
root@benchify-fixer-staging-api-bcb476df9-2jmdm:/app# exit
exit
```

Interestingly, I did not manage to reproduce the same issue locally on my mac:

```
> echo "print('hello world')" > foo.py; TMPDIR='doesnt_exist/' uv run foo.py
hello world
```

### Platform

Linux 5.10.244-240.965.amzn2.x86_64 x86_64 GNU/Linux

### Version

0.7.5

### Python version

3.13.9

---

_Label `bug` added by @maxvonhippel on 2025-10-21 21:22_

---

_Comment by @zanieb on 2025-10-21 23:07_

That's peculiar. I imagine we'd create the parent directories regardless of platform.. ?

---

_Renamed from "`uv run` does not work if `TEMPDIR` does not exist" to "`uv run` does not work if `TMPDIR` does not exist" by @konstin on 2025-10-22 07:57_

---

_Comment by @konstin on 2025-10-22 08:01_

What's in the `/app` directory, can you create a reproducible example from this? I tried some `TMPDIR=/wrong uv` commands but they all passed on my machine.

---

_Label `needs-mre` added by @konstin on 2025-10-22 08:01_

---

_Comment by @mlanett on 2025-11-06 00:53_

I've been having a VERY similar problem where using a non-root user causes uv to fail to run because it does not have write permission to /tmp in our container.

This happens despite using all of:
UV_FROZEN=1
UV_NO_CACHE=1
UV_NO_SYNC=1

It seems to create a 0-byte lock file e.g. `uv-41b9c140d569f2b9.lock`

---

_Comment by @zanieb on 2025-11-06 04:04_

@mlanett please open a new issue with a reproduction, that's not the same thing as reported here.

---
