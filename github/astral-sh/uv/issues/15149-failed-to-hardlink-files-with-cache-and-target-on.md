---
number: 15149
title: Failed to hardlink files with cache and target on same filesystem
type: issue
state: open
author: mpetruc
labels:
  - question
assignees: []
created_at: 2025-08-07T20:55:51Z
updated_at: 2025-08-09T00:15:11Z
url: https://github.com/astral-sh/uv/issues/15149
synced_at: 2026-01-07T13:12:19-06:00
---

# Failed to hardlink files with cache and target on same filesystem

---

_Issue opened by @mpetruc on 2025-08-07 20:55_

### Question

```
export UV_CACHE_DIR='/home/user1/data/.cache/uv'
export UV_TOOL_BIN_DIR='/home/user1/data/uv/tools'
uv tool update-shell

(base) [user1@login ~]$ df -T /home/user1/data/.cache/uv
Filesystem                             Type    1K-blocks         Used   Available Use% Mounted on
xxx:/mnt/yyy/data nfs4 150323855360 114649980928 35673874432  77% /mnt/yyy/data
(base) [user1@login ~]$ df -T /home/user1/data/uv/tools
Filesystem                             Type    1K-blocks         Used   Available Use% Mounted on
xxx:/mnt/yyy/data nfs4 150323855360 114650914816 35672940544  77% /mnt/yyy/data

df -T ~/.local/bin/uv
Filesystem                             Type   1K-blocks        Used   Available Use% Mounted on
xxx:/mnt/yyy/home nfs4 64424509440 21567635456 42856873984  34% /cluster/yyy/home

uv tool install some_tool
Resolved 32 packages in 552ms
░░░░░░░░░░░░░░░░░░░░ [0/32] Installing wheels...                                                                    
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
```

In addition to cache and target being on the same filesystem, does the uv binary have to be on the same file system as well?

Thanks.

### Platform

AlmaLinux 8.10 (Cerulean Leopard)

### Version

0.6.1

---

_Label `question` added by @mpetruc on 2025-08-07 20:55_

---

_Comment by @zanieb on 2025-08-07 20:59_

We need to know your platform and uv version. Share verbose logs too. See #9452.

---

_Comment by @zanieb on 2025-08-07 21:00_

You're using an NFS? Does your NFS support hard links?

---

_Comment by @mpetruc on 2025-08-08 20:58_

Don't know if NSF supports hard links or not. The warning did not say anything about the _type_ of filesystem, just that if the cache and target are on different file systems this might happen. They are not. However, it still "Failed to hardlink files".
Also,
Platform
AlmaLinux 8.10 (Cerulean Leopard)

UV Version
0.6.1

thanks.

---

_Comment by @zanieb on 2025-08-09 00:15_

nfs4 _should_ support hardlinks, I think we'll need to dig into why specifically the hard links are failing on your file system. Using multiple file systems is just the common case for them to fail.

Can you share more information about your file system? What's the underlying file system on the server?

---
