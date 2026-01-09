---
number: 18373
title: Failed to rename cache file when trying to run ruff
type: issue
state: closed
author: zoeelkins
labels: []
assignees: []
created_at: 2025-05-29T15:43:30Z
updated_at: 2025-05-29T19:06:27Z
url: https://github.com/astral-sh/ruff/issues/18373
synced_at: 2026-01-07T13:12:16-06:00
---

# Failed to rename cache file when trying to run ruff

---

_Issue opened by @zoeelkins on 2025-05-29 15:43_

### Summary

This issue seems to be similar/related to [Issue #9490](https://github.com/astral-sh/ruff/issues/9490).

I am using Databricks and install ruff and uv via pip, particularly using Databricks' magic command (`%pip`).
```
%pip install uv
%pip install ruff
%restart_python
```
I checked my version of ruff with this command:
```
$ uv run --active ruff --version
   ruff 0.11.12
```

When I run a check on my directory using ruff, I find the following error:
```
$ uv run --active ruff --isolated check file.ipynb
ruff failed
  Cause: Failed to rename temporary cache file to /Workspace/Users/ze@email.com/sandbox/.ruff_cache/0.11.12/16841316555859280395
  Cause: failed to persist temporary file: No such file or directory (os error 2)
  Cause: No such file or directory (os error 2)
```

Any help is appreciated! Thanks!

### Version

ruff 0.11.12

---

_Comment by @MichaReiser on 2025-05-29 16:51_

Hmm. I'm unfamiliar with databrick

Does the directory /Workspace/Users/ze@email.com/sandbox/.ruff_cache/ exist?

---

_Comment by @zoeelkins on 2025-05-29 17:10_

@MichaReiser yes it does -- as does the 0.11.12 folder! 

Databricks is an "all in one"-type data science solution. Put more simply, it's an .ipynb GUI with datalake & SQL bells & whistles. When you want to execute code, you establish a connection to a compute cluster (unix based), install dependencies, and run the code. 

---

_Comment by @ntBre on 2025-05-29 18:51_

I think we're using this [`persist`](https://docs.rs/tempfile/latest/tempfile/struct.NamedTempFile.html#method.persist) method from the `tempfile` crate here, which says

> Note: Temporary files cannot be persisted across filesystems. Also neither the file contents nor the containing directory are synchronized, so the update may not yet have reached the disk when persist returns.

Is it possible that your system's temporary directory is on a different filesystem than `/Workspace`? It sounds like `/Workspace` could even be on a remote filesystem.

As a workaround, you should be able to disable the caching with the `--no-cache` flag.

---

_Comment by @zoeelkins on 2025-05-29 19:06_

@ntBre thank you! That makes sense. Because, yes -- `/Workspace` is a remote filesystem separate from the cluster's temporary directory ([DBFS](https://docs.databricks.com/aws/en/files)). I hadn't even considered that. Passing the `--no-cache` flag resolved the issue!

For reference, here is the command that worked on Databricks:
```
$ uv run --active ruff check --no-cache file.ipynb
```

---

_Closed by @zoeelkins on 2025-05-29 19:06_

---
