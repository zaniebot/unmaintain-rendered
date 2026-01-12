```yaml
number: 15417
title: Relative path inconsistency in uv.lock for cache_dir at different depths
type: issue
state: open
author: xcorp
labels:
  - bug
assignees: []
created_at: 2025-08-21T14:12:14Z
updated_at: 2025-09-06T02:48:30Z
url: https://github.com/astral-sh/uv/issues/15417
synced_at: 2026-01-12T16:02:10Z
```

# Relative path inconsistency in uv.lock for cache_dir at different depths

---

_@xcorp_

### Summary

**Summary:**

When two people check out the same project at different paths relative to `UV_CACHE_DIR`, the relative paths written in `uv.lock` differ for each user. This is because the paths are currently calculated relative to the project directory rather than the cache directory. Although the paths will be recalculated as needed and do not prevent the project from working, this inconsistency leads to unnecessary modifications in the `uv.lock` file. As a result, Git will show the lock file as changed, even when there are no real updates, such as new package versions, that need to be committed. This creates confusion and noise in version control, making it harder to track legitimate changes to dependencies.

**Minimal reproducible example:**
1. Person A checks out the project at `/home/alice/project` and has `UV_CACHE_DIR` at `/home/alice/.cache/uv`.
2. Person B checks out the same project at `/Users/bob/dev/project` and has `UV_CACHE_DIR` at `/Users/bob/.cache/uv`.
3. Run `uv` in both environments to generate a `uv.lock` file (e.g., `uv pip install -r requirements.txt`).
4. Observe that the relative paths written in `uv.lock` differ based on the depth of the project relative to the cache directory.
5. Use `git status` and see that `uv.lock` is marked as modified, even though no package changes occurred.

**Expected behavior:**
Relative paths in `uv.lock` should always be resolved relative to `UV_CACHE_DIR`, not the project directory, so that the lock file is consistent regardless of where the project is checked out.

**Actual behavior:**
Relative paths in `uv.lock` are currently calculated relative to the project directory, resulting in inconsistencies when the project is checked out at different depths compared to the cache directory. This leads to unnecessary changes being detected by Git.

**Proposed solution:**
Keep all paths in the `uv.lock` file relative to `UV_CACHE_DIR`, not the project directory.

### Platform

Ubuntu 24.04.1 LTS (Noble Numbat), x86_64  Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.8.4

### Python version

Python 3.11.10

---

_Label `bug` added by @xcorp on 2025-08-21 14:12_

---

_Comment by @charliermarsh on 2025-08-21 23:26_

Are you able to create a minimal reproduction? Like, what requirements need to be present in the projects to trigger this? I don't think the lockfile should ever contain paths relative to the cache -- the paths are supposed to be relative to the root of the project directory, so a reproducible example would be helpful.

---

_Label `needs-mre` added by @charliermarsh on 2025-08-21 23:26_

---

_Comment by @xcorp on 2025-08-25 09:48_

Minimal repo can be found at https://github.com/xcorp/uv-issue-15417
The problem only occurs if I have packages with "external" sources, like github, so if I remove the package vsphere-automation-sdk there are no cache-paths at all in the lock file. I know that that specific package is a mess, but I also have internally hosted packges that was removed from the example dependency list.

---

_Comment by @xcorp on 2025-09-04 05:44_

Don't know if you saw my repo for reproduction @charliermarsh, sorry about the tagging if you already saw it.

---

_Comment by @charliermarsh on 2025-09-04 14:53_

Thanks! We'll take a look.

---

_Label `needs-mre` removed by @charliermarsh on 2025-09-04 14:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-04 16:36_

---

_Comment by @charliermarsh on 2025-09-06 02:48_

Ahh thanks. I understand. This would be solved by https://github.com/astral-sh/uv/pull/10072.

---
