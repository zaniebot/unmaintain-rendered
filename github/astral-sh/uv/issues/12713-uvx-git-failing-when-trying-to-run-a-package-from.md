---
number: 12713
title: uvx git+.. failing when trying to run a package from a subfolder.
type: issue
state: closed
author: RonanKMcGovern
labels:
  - bug
assignees: []
created_at: 2025-04-07T12:39:18Z
updated_at: 2025-10-14T10:31:44Z
url: https://github.com/astral-sh/uv/issues/12713
synced_at: 2026-01-07T13:12:18-06:00
---

# uvx git+.. failing when trying to run a package from a subfolder.

---

_Issue opened by @RonanKMcGovern on 2025-04-07 12:39_

### Summary

Try to run a package from a subfolder with:
```bash
uvx git+https://github.com/RonanKMcGovern/uv-subfolder#subfolder=subfolder-test
```
See https://github.com/RonanKMcGovern/uv-subfolder for the minimal repro example.

Error:
```
uvx git+https://github.com/RonanKMcGovern/uv-subfolder#subfolder-test

    Updated https://github.com/RonanKMcGovern/uv-subfolder (cd481f4bcf717
  × Failed to resolve `--with` requirement
  ╰─▶ /Users/ronanmcgovern/.cache/uv/git-v0/checkouts/536243fbda2870a9/cd481f4
      does not appear to be a Python project, as neither
      `pyproject.toml` nor `setup.py` are present in the directory
```
Note: I ran `uv clean` before running the command.

Other notes:
- I'm also running a more complex private example and getting this error:
```
× Failed to resolve `--with` requirement
  ╰─▶ Git operation failed
```
but not getting the note on the toml or setup.py not being present.

### Platform

Darwin 24.3.0 arm64

### Version

uv 0.6.12 (Homebrew 2025-04-02)

### Python version

Python 3.12.4

---

_Label `bug` added by @RonanKMcGovern on 2025-04-07 12:39_

---

_Comment by @charliermarsh on 2025-04-07 14:40_

I think there might be a more general bug here whereby we're treating `#subfolder=subfolder-test` as a comment in some cases? You can put this in a `requirements.txt` and `uv pip install -r requirements.txt` will show the same error:

```
git+https://github.com/RonanKMcGovern/uv-subfolder#subfolder=subfolder-test
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-07 14:41_

---

_Comment by @charliermarsh on 2025-04-07 14:42_

Oh sorry. You want `subdirectory=subfolder-test`. I think that's just a typo.

---

_Comment by @charliermarsh on 2025-04-07 14:42_

`uvx git+https://github.com/RonanKMcGovern/uv-subfolder#subdirectory=subfolder-test` works.

---

_Comment by @RonanKMcGovern on 2025-04-07 15:56_

Thanks very much @charliermarsh - bad typo on my part. Yes, I can run that repro now.

I've expanded the repro (more complex repro [here](https://github.com/RonanKMcGovern/doc-convert)) to try and get to my core issue, and this more complex repo works correctly (I've tried adding in parallel subfolders with toml files to see if that could cause an issue, but that doesn't seem to break things). So I need to dig more on what is causing the issue in my private repo:
```
uvx git+https://github.com/TrelisResearch/ADVANCED-inference@doc-mcp#subdirectory=trelis-mcp/doc-convert 
   Updating https://github.com/TrelisResearch/ADVANCED-inference (doc-mcp)                                                                                 × Failed to resolve `--with` requirement
  ╰─▶ Git operation failed
```
I'll reopen another issue if I'm able to repro a specific problem. Thanks

---

_Closed by @RonanKMcGovern on 2025-04-07 15:56_

---

_Comment by @RonanKMcGovern on 2025-04-08 07:58_

Hi @charliermarsh , I went fresh-brain on the problem this morning.

Problem was that I had [this repo](https://github.com/modelcontextprotocol/servers) cloned in a subdirectory, and that was causing interference with uvx.

Thanks for the help earlier. Appreciated it.

---

_Comment by @charliermarsh on 2025-04-08 12:16_

No worries, thanks for following up!

---

_Comment by @RonanKMcGovern on 2025-05-01 17:16_

Howdy @charliermarsh , this is quite an edge case, and you know much better the right standards here, but just a heads up that if a git repo has got any kind of submodule (e.g. another GitHub repo in it) that seems to make it hard for uvx to find packages. Probably is bad practise sometimes have a repo in a repo, but can happen.

---

_Referenced in [oraios/serena#296](../../oraios/serena/issues/296.md) on 2025-07-11 08:13_

---

_Comment by @m-aciek on 2025-10-14 09:16_

For me it happens when I try to install from private GitHub repository. Switching to SSH solves the problem. It would be nice if the error message would be more descriptive for this case. Debug log:
```
DEBUG Updating Git source `https://github.com/company/repo`
   Updating https://github.com/company/repo (HEAD)
DEBUG Attempting GitHub fast path for: https://api.github.com/repos/company/repo/commits/HEAD
DEBUG Failed to check GitHub HTTP status client error (404 Not Found) for url (https://api.github.com/repos/company/repo/commits/HEAD)
DEBUG Performing a Git fetch for: https://github.com/company/repo
DEBUG Released lock at `/Users/m-aciek/.cache/uv/git-v0/locks/33b3bb97da73f80e`
  × Failed to resolve `--with` requirement
  ╰─▶ Git operation failed
```

---

_Comment by @konstin on 2025-10-14 10:31_

@m-aciek This is a different problem, please open a new issue for it.

---

_Referenced in [astral-sh/uv#16303](../../astral-sh/uv/issues/16303.md) on 2025-10-14 21:54_

---
