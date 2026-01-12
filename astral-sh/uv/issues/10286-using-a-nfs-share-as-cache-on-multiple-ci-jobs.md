```yaml
number: 10286
title: Using a NFS share as cache on multiple CI jobs results in lock timeouts
type: issue
state: closed
author: RonNabuurs
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-01-03T14:01:16Z
updated_at: 2025-01-03T16:59:59Z
url: https://github.com/astral-sh/uv/issues/10286
synced_at: 2026-01-12T16:00:10Z
```

# Using a NFS share as cache on multiple CI jobs results in lock timeouts

---

_@RonNabuurs_

# Goal
I want to reduce network usage on my local runners and increase performance. 

# Approach
I've created a local cache in my server rack which is mounted to all my runners as an NFS share. I can access this share in all the jobs on my GitLab CI Runners. 

I've mounted a directory in the NFS share as the uv cache. And use the `UV_CACHE_DIR` variable to configure uv in the pipeline.

```yaml
variables:
  UV_CACHE_DIR: /mnt/shared-cache/python/uv-cache
```

# Result

Whenever I've multiple jobs running concurrently I get lock wait timeouts.
E.g.:
```
warning: Waiting to acquire lock for /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/reportlab/3.6.13 (lockfile: /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/reportlab/3.6.13/.lock)
warning: Waiting to acquire lock for /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/pymongo/4.7.3 (lockfile: /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/pymongo/4.7.3/.lock)
warning: Waiting to acquire lock for /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/svglib/1.5.1 (lockfile: /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/svglib/1.5.1/.lock)
warning: Waiting to acquire lock for /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/mysqlclient/2.1.1 (lockfile: /mnt/shared-cache/python/uv-cache/built-wheels-v3/index/0ab6a37ad624c78f/mysqlclient/2.1.1/.lock)
```  

Is this an issue in my configuration or is this something else?

---

_Label `bug` added by @charliermarsh on 2025-01-03 15:30_

---

_Comment by @charliermarsh on 2025-01-03 15:30_

It's probably a bug though it may be hard for us to fix without being able to reproduce it.

---

_Label `needs-mre` added by @charliermarsh on 2025-01-03 15:30_

---

_Comment by @charliermarsh on 2025-01-03 15:31_

Separately, I think you're on a fairly old version of uv (we renamed `built-wheels` a while ago). So you may want to upgrade first.

---

_Comment by @RonNabuurs on 2025-01-03 16:13_

Mmm, interesting. I've updated to the latest uv (0.5.14), but now I get an error a out hard linking. Which is very interesting to me, because previously I did not have any issue with this. After the weekend I can try updating the link mode to symlink and see if it solves the issue.

```
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
```

---

_Comment by @charliermarsh on 2025-01-03 16:14_

Critically that's not an error -- just a warning. And it does make sense, because we can't hard-link across drives (and your cache and virtual environment are likely on separate drives now). I suspect the same thing was happening before, we just didn't show the warning.

---

_Comment by @RonNabuurs on 2025-01-03 16:44_

It seems to be like it indeed. In that case this issue can be closed and unfortunately using symlink does not really increase the performance across the jobs it seems. 

---

_Closed by @zanieb on 2025-01-03 16:59_

---
