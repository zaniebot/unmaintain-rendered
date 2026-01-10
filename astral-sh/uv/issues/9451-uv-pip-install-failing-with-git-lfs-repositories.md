---
number: 9451
title: "`uv pip install` failing with Git LFS repositories"
type: issue
state: closed
author: dimk90
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-11-26T20:18:06Z
updated_at: 2024-12-02T19:28:09Z
url: https://github.com/astral-sh/uv/issues/9451
synced_at: 2026-01-10T01:24:41Z
---

# `uv pip install` failing with Git LFS repositories

---

_Issue opened by @dimk90 on 2024-11-26 20:18_

## Description

Installation from a repository with LFS content fails with a smudge error, e.g.
```bash
uv venv -p 3.11
uv pip install "git+https://github.com/makarovdi/uplot.git@main"
```
the output:
```
Updating https://github.com/makarovdi/uplot.git (main)
error: Git operation failed
  Caused by: process didn't exit successfully: `D:\Workspace\sdk\Git\cmd\git.exe reset --hard dbc97a068bb6128833a4a7cc3d1e15649a03e3c2` (exit code: 128)
--- stderr
Downloading gallery/asset/hvline.png (184 KB)
Error downloading object: gallery/asset/hvline.png (e0ceff1): Smudge error: Error downloading gallery/asset/hvline.png (e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c): error transferring "e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c": [0] remote missing object e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c

Errors logged to 'D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\lfs\logs\20241126T212020.8355407.log'.
Use `git lfs logs last` to view the log.
error: external filter 'git-lfs filter-process' failed
fatal: gallery/asset/hvline.png: smudge filter lfs failed
```  

<details>
   <summary><b>Verbose Output</b></summary>

```
$ GIT_TRACE=1 uv pip install -v "git+https://github.com/makarovdi/uplot.git@main"

DEBUG uv 0.5.4 (c62c83c37 2024-11-20)
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.11.10-windows-x86_64-none` at `D:\Workspace\sdk\msys2\tmp\.venv\Scripts\python.exe` (virtual environment)
DEBUG Using Python 3.11.10 environment at .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: git+https://github.com/makarovdi/uplot.git@main
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: https://github.com/makarovdi/uplot.git
DEBUG Acquired lock for `https://github.com/makarovdi/uplot`
DEBUG Updating Git source `https://github.com/makarovdi/uplot.git`
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/makarovdi/uplot/commits/main
DEBUG Performing a Git fetch for: https://github.com/makarovdi/uplot.git
DEBUG reset D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0 to dbc97a068bb6128833a4a7cc3d1e15649a03e3c2
DEBUG Released lock at `D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\locks\b218c100bb0184e1`
DEBUG Released lock at `D:\Workspace\sdk\msys2\tmp\.venv\.lock`
error: Git operation failed
  Caused by: process didn't exit successfully: `D:\Workspace\sdk\Git\cmd\git.exe reset --hard dbc97a068bb6128833a4a7cc3d1e15649a03e3c2` (exit code: 128)
--- stderr
21:21:37.010198 exec-cmd.c:243          trace: resolved executable dir: D:/Workspace/sdk/Git/mingw64/bin
21:21:37.025993 git.c:465               trace: built-in: git reset --hard dbc97a068bb6128833a4a7cc3d1e15649a03e3c2
21:21:37.025993 run-command.c:657       trace: run_command: 'git-lfs filter-process'
21:21:37.451457 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'rev-parse' '--git-dir' '--show-toplevel'
21:21:37.520338 trace git-lfs: exec: git 'rev-parse' '--is-bare-repository'
21:21:37.567338 trace git-lfs: exec: git 'config' '--includes' '--local' 'lfs.repositoryformatversion'
21:21:37.614976 trace git-lfs: exec: git 'config' '--includes' '--replace-all' 'lfs.repositoryformatversion' '0'
21:21:37.679131 trace git-lfs: exec: git 'config' '--includes' '-l'
21:21:37.727555 trace git-lfs: exec: git 'rev-parse' '--is-bare-repository'
21:21:37.784939 trace git-lfs: exec: git 'config' '--includes' '-l' '--blob' ':.lfsconfig'
21:21:37.849423 trace git-lfs: exec: git 'config' '--includes' '-l' '--blob' 'HEAD:.lfsconfig'
21:21:37.925630 trace git-lfs: Install hook: pre-push, force=false, path=D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\hooks\pre-push
21:21:37.926228 trace git-lfs: Install hook: post-checkout, force=false, path=D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\hooks\post-checkout
21:21:37.926767 trace git-lfs: Install hook: post-commit, force=false, path=D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\hooks\post-commit
21:21:37.927303 trace git-lfs: Install hook: post-merge, force=false, path=D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\hooks\post-merge
21:21:37.927303 trace git-lfs: Initialize filter-process
21:21:37.928466 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'rev-parse' 'HEAD' '--symbolic-full-name' 'HEAD'
21:21:37.973273 trace git-lfs: Error loading current ref: Git can't resolve ref: "HEAD"
21:21:37.973897 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'rev-parse' '--git-dir'
21:21:38.022759 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'remote'
21:21:38.068208 trace git-lfs: tq: running as batched queue, batch size of 100
21:21:38.069606 trace git-lfs: filepathfilter: accepting "gallery/asset/hvline.png"
21:21:38.071303 trace git-lfs: filepathfilter: accepting "gallery/asset/legend.png"
21:21:38.071832 trace git-lfs: filepathfilter: accepting "gallery/asset/mpl-example.png"
21:21:38.073024 trace git-lfs: filepathfilter: accepting "gallery/asset/plot-3d.png"
21:21:38.073597 trace git-lfs: filepathfilter: accepting "gallery/asset/plot.png"
21:21:38.074655 trace git-lfs: filepathfilter: accepting "gallery/asset/plotly5-example.png"
21:21:38.075283 trace git-lfs: filepathfilter: accepting "gallery/asset/plugin.png"
21:21:38.076339 trace git-lfs: filepathfilter: accepting "gallery/asset/scatter-3d.png"
21:21:38.076915 trace git-lfs: filepathfilter: accepting "gallery/asset/scatter.png"
21:21:38.077996 trace git-lfs: filepathfilter: accepting "gallery/asset/surface3d.png"
Updating files: 100% (54/54), done.
21:21:38.095627 trace git-lfs: tq: sending batch of size 10
21:21:38.095627 trace git-lfs: tq: starting transfer adapter "lfs-standalone-file"
21:21:38.097282 trace git-lfs: exec: sh '-c' 'git-lfs standalone-file'
21:21:39.048814 trace git-lfs: tq: running as batched queue, batch size of 100
21:21:39.049376 trace git-lfs: filepathfilter: accepting "gallery/asset/hvline.png"
Downloading gallery/asset/hvline.png (184 KB)
21:21:39.049376 trace git-lfs: tq: running as batched queue, batch size of 100
21:21:39.049376 trace git-lfs: tq: sending batch of size 1
21:21:39.049376 trace git-lfs: tq: starting transfer adapter "lfs-standalone-file"
21:21:39.052235 trace git-lfs: exec: sh '-c' 'git-lfs standalone-file'
Error downloading object: gallery/asset/hvline.png (e0ceff1): Smudge error: Error downloading gallery/asset/hvline.png (e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c): error transferring "e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c": [0] remote missing object e0ceff1ec4965d6141a39c652807adde9c67d5019aff2cd7dae85390a740086c
21:21:40.020979 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'rev-parse' '--git-dir'
21:21:40.092706 trace git-lfs: exec: git '-c' 'filter.lfs.smudge=' '-c' 'filter.lfs.clean=' '-c' 'filter.lfs.process=' '-c' 'filter.lfs.required=false' 'remote'

Errors logged to 'D:\Workspace\sdk\msys2\home\dmitry\.cache\uv\git-v0\checkouts\b218c100bb0184e1\dbc97a0\.git\lfs\logs\20241126T212140.0137644.log'.
Use `git lfs logs last` to view the log.
error: external filter 'git-lfs filter-process' failed
fatal: gallery/asset/hvline.png: smudge filter lfs failed
```
</details>

## Versions

`Windows 11 Pro 24H2`  + `MSYS2`
`uv 0.5.4 (c62c83c37 2024-11-20)`  


## Related issues

- #3312 is related probably.

---

_Comment by @charliermarsh on 2024-11-26 20:22_

How does this differ from https://github.com/astral-sh/uv/issues/3312?

---

_Label `question` added by @charliermarsh on 2024-11-26 20:28_

---

_Comment by @dimk90 on 2024-12-02 18:16_

> How does this differ from [#3312](https://github.com/astral-sh/uv/issues/3312)?

The main difference is that the original question in 3312 focused on `Rye` and `git2`.
After reading the entire discussion, I found a similar example with `uv pip install` and its output.
It seems this is a duplicate. Apologies for that - I should have read 3312 more carefully.

---

_Comment by @zanieb on 2024-12-02 19:28_

No problem! It happens. Thanks for looking into it.

---

_Closed by @zanieb on 2024-12-02 19:28_

---

_Label `duplicate` added by @zanieb on 2024-12-02 19:28_

---
