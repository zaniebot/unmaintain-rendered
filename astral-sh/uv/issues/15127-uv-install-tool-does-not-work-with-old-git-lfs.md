```yaml
number: 15127
title: "`uv install tool` does not work with old `git-lfs`"
type: issue
state: open
author: avaldebe
labels:
  - bug
assignees: []
created_at: 2025-08-07T09:38:22Z
updated_at: 2025-08-07T09:38:22Z
url: https://github.com/astral-sh/uv/issues/15127
synced_at: 2026-01-12T16:02:04Z
```

# `uv install tool` does not work with old `git-lfs`

---

_@avaldebe_

### Summary

# Summary

I'm in a similar situation than #14173 (the minimal example is from that issue).
I want to install a tool from a GitHub repository that uses `git-lfs`,
and needs those `git-lfs` files to work correctly.

## Minimal Reproducible Example

The [repo](https://github.com/asmith26/debug-uvx-gitlfs/) from #14173 main branch **has git-lfs files**,
which causes the `tool install` to fail

```console
$ git lfs install --skip-repo
$ UV_GIT_LFS=1 uv -v tool install "git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git"
DEBUG uv 0.8.5
DEBUG Searching for default Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `/home/sby/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.5-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.5-linux-x86_64-gnu` at `/home/sby/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13` (managed installations)
DEBUG Ignoring non-package name `git+ssh://git` in command
DEBUG Using request timeout of 30s
DEBUG Attempting GitHub fast path for: git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git
DEBUG Querying GitHub for commit at: https://api.github.com/repos/asmith26/debug-uvx-gitlfs/commits/HEAD
DEBUG Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/asmith26/debug-uvx-gitlfs/578ecc3acbb09897dcb5abba966a7530cb6a9ebf/pyproject.toml
DEBUG Found static metadata via GitHub fast path for: git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git
DEBUG Acquired lock for `/home/sby/.local/share/uv/tools`
DEBUG Checking for Python environment at: `/home/sby/.local/share/uv/tools/my-package`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
DEBUG Adding direct dependency: my-package*
DEBUG Searching for a compatible version of my-package @ git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git (*)
DEBUG Tried 1 versions: my-package 1
DEBUG marker environment resolution took 0.001s
Resolved 1 package in 57ms
DEBUG Creating environment for tool `my-package`: /home/sby/.local/share/uv/tools/my-package
DEBUG Assessing Python executable as base candidate: /home/sby/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
DEBUG Using base executable for virtual environment: /home/sby/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: my-package @ git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git@578ecc3acbb09897dcb5abba966a7530cb6a9ebf
DEBUG Fetching source distribution from Git: ssh://git@github.com/asmith26/debug-uvx-gitlfs.git
DEBUG Acquired lock for `ssh://github.com/asmith26/debug-uvx-gitlfs`
DEBUG Using existing Git source `ssh://git@github.com/asmith26/debug-uvx-gitlfs.git`
DEBUG Reset /home/sby/.cache/uv/git-v0/checkouts/44eeb432cca41142/578ecc3 to 578ecc3acbb09897dcb5abba966a7530cb6a9ebf
DEBUG Released lock at `/home/sby/.cache/uv/git-v0/locks/44eeb432cca41142`
DEBUG Failed to sync environment; removing `my-package`
DEBUG Deleting environment for tool `my-package` at /home/sby/.local/share/uv/tools/my-package
  × Failed to download and build `my-package @ git+ssh://git@github.com/asmith26/debug-uvx-gitlfs.git@578ecc3acbb09897dcb5abba966a7530cb6a9ebf`
  ├─▶ Git operation failed
  ╰─▶ process didn't exit successfully: `/usr/local/apps/git/2.48.1/bin/git reset --hard 578ecc3acbb09897dcb5abba966a7530cb6a9ebf` (exit status: 128)
      --- stderr

      hint: The remote resolves to a file:// URL, which can only work with a
      hint: standalone transfer agent.  See section "Using a Custom Transfer Type
      hint: without the API server" in custom-transfers.md for details.
      Downloading src/my_package/git-lfs.txt (25 B)

      hint: The remote resolves to a file:// URL, which can only work with a
      hint: standalone transfer agent.  See section "Using a Custom Transfer Type
      hint: without the API server" in custom-transfers.md for details.
      Error downloading object: src/my_package/git-lfs.txt (9dd7ca1): Smudge error: Error downloading src/my_package/git-lfs.txt (9dd7ca19a3305d52c11ed9ed2121b1c270b99aa3cd1e0ebb92270e7b44f5829d): batch request: missing protocol:
      "file:///home/sby/.cache/uv/git-v0/db/44eeb432cca41142/.git/info/lfs"

      Errors logged to /etc/ecmwf/nfs/dh1_home_b/sby/.cache/uv/git-v0/checkouts/44eeb432cca41142/578ecc3/.git/lfs/logs/20250807T092626.698267205.log
      Use `git lfs logs last` to view the log.
      error: external filter 'git-lfs filter-process' failed
      fatal: src/my_package/git-lfs.txt: smudge filter lfs failed

DEBUG Released lock at `/home/sby/.local/share/uv/tools/.lock`
```

The `git` version I have available is quite recent (2.48.1 from Jan 2025) but `git-lfs` is quite old (2.6.1 from Dec 2018). Here is my `git lfs env`

```console
$ git lfs env
git-lfs/2.6.1 (GitHub; linux amd64; go 1.11.2; git dc072c3e)
git version 2.48.1

LocalWorkingDir=
LocalGitDir=
LocalGitStorageDir=
LocalMediaDir=lfs/objects
LocalReferenceDirs=
TempDir=lfs/tmp
ConcurrentTransfers=3
TusTransfers=false
BasicTransfersOnly=false
SkipDownloadErrors=false
FetchRecentAlways=false
FetchRecentRefsDays=7
FetchRecentCommitsDays=0
FetchRecentRefsIncludeRemotes=true
PruneOffsetDays=3
PruneVerifyRemoteAlways=false
PruneRemoteName=origin
LfsStorageDir=lfs
AccessDownload=none
AccessUpload=none
DownloadTransfers=basic
UploadTransfers=basic
GIT_EXEC_PATH=/usr/local/apps/git/2.48.1/libexec/git-core
GIT_VERSION=2.48.1
git config filter.lfs.process = "git-lfs filter-process"
git config filter.lfs.smudge = "git-lfs smudge -- %f"
git config filter.lfs.clean = "git-lfs clean -- %f"
```

### Platform

Linux 4.18.0-553.52.1.el8_10.x86_64 x86_64 GNU/Linux

### Version

uv 0.8.5

### Python version

Python 3.13.5

---

_Label `bug` added by @avaldebe on 2025-08-07 09:38_

---
