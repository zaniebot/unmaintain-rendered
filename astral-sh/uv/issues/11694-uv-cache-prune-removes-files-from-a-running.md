---
number: 11694
title: "uv cache prune removes files from a running process executed with `uvx`"
type: issue
state: closed
author: jezekra1
labels:
  - bug
assignees: []
created_at: 2025-02-21T10:28:16Z
updated_at: 2025-11-28T15:18:07Z
url: https://github.com/astral-sh/uv/issues/11694
synced_at: 2026-01-10T01:25:09Z
---

# uv cache prune removes files from a running process executed with `uvx`

---

_Issue opened by @jezekra1 on 2025-02-21 10:28_

### Summary

When a tool is executing for a long period of time, for example `uvx mlflow server`,
running `uv cache prune` will remove its files while still running!

This is a screenshot from from htop:
![Image](https://github.com/user-attachments/assets/016bab73-46b9-4a7f-a616-5ad293e15e28)

However `uv cache prune` will remove it:

```
❯ ls /Users/radek/.cache/uv/archive-v0/yl7-ET-Ix2_OI5icDQcWe
CACHEDIR.TAG bin          lib          pyvenv.cfg   share
❯ uv cache prune
Pruning cache at: /Users/radek/.cache/uv
Removed 12436 files (465.6MiB)
❯ ls /Users/radek/.cache/uv/archive-v0/yl7-ET-Ix2_OI5icDQcWe
ls: /Users/radek/.cache/uv/archive-v0/yl7-ET-Ix2_OI5icDQcWe: No such file or directory
```

But the `mflow` server process is still running! This will cause any runtime imports to fail. If this happens during the process startup, it fails inevitably.

This is also contrary to the statement in docs:

> uv cache prune removes all unused cache entries. For example, the cache directory may contain entries created in previous uv versions that are no longer necessary and can be safely removed. uv cache prune is safe to run periodically, to keep the cache directory clean.

https://docs.astral.sh/uv/concepts/cache/#clearing-the-cache

---
**Naive fix idea**

Add a new cache bucket which would use `pid` of the running process `~/.cache/uv/uvx-process-v1/{pid}/{symlink}`
When pruning cache uv could check whether the process with pid still exists and if not - remove the directory.

---

**Related feature request**:

Also it would be nice to have a way to prune old versions of the tool only and keep the latest one (the one that would be used by `uvx <tool>` as per docs, we could have `--keep-tools` flag to `uv cache prune` or something like that:

> uvx will use the latest available version of the requested tool on the first invocation. After that, uvx will use the cached version of the tool unless a different version is requested, the cache is pruned, or the cache is refreshed.

---

**Context**:

We're trying to create a platform which spawns processes using `uvx`:

https://github.com/i-am-bee/beeai/blob/85d970a5b7436f742bc85ed9ab6e324bf91bbeed/apps/beeai-server/src/beeai_server/domain/model.py#L203

However it leaves a lot of files on the filesystem potentially forever with no way to clean them because `uv cache prune` breaks the running processes.

Thanks for all the amazing work!


### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.2 (6d3614eec 2025-02-19)

### Python version

Python 3.11.11

---

_Label `bug` added by @jezekra1 on 2025-02-21 10:28_

---

_Referenced in [astral-sh/uv#12003](../../astral-sh/uv/issues/12003.md) on 2025-03-07 16:11_

---

_Comment by @charliermarsh on 2025-11-28 15:18_

I think this is likely no longer true given that we hold a lock on the cache itself.

---

_Closed by @charliermarsh on 2025-11-28 15:18_

---
