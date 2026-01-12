```yaml
number: 17200
title: Remove .gitignore from project file list
type: pull_request
state: closed
author: bensgilbert
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-12-20T23:53:08Z
updated_at: 2025-12-21T21:21:30Z
url: https://github.com/astral-sh/uv/pull/17200
synced_at: 2026-01-12T16:12:39Z
```

# Remove .gitignore from project file list

---

_@bensgilbert_

Removed .gitignore from the list of created files.

No issue created as quite a small change. Running `uv init` does **not** create a `.gitignore` file, therefore this list is slightly misleading.

~~Test Plan~~

---

_Comment by @zanieb on 2025-12-21 01:25_

It does create the file though, unless you're already in a `.git` repository.

---

_Comment by @bensgilbert on 2025-12-21 02:23_

ah you're correct, my apologies, I should have created an issue first and more importantly, taken the time to read the codebase rather than jump to a pr, lesson learnt!
this behaviour appears undocumented (as far as I could find) and also counterintuitive to me, a .gitignore file would be useful when also in a git repository if there isn't one already? alternatively, the wording in the docs could be changed to something along the lines of:


uv will create the following files:
```text
├── .gitignore - if not already in a git repository
├── .python-version
...
```

---

_Comment by @zanieb on 2025-12-21 03:15_

I don't think it's worth adding that nuance to the "guide", it's intended to be an overview for getting started not comprehensive documentation. We could mention it in the `--vcs` reference documentation. We could also consider `--ignore-file` / `--no-ignore-file` options to toggle this separately from initializing the vcs.

Your other suggestion is already being discussed in https://github.com/astral-sh/uv/issues/11655 — I think it's probably reasonable to change our behavior to create it in existing repositories if there's not a parent `.gitignore`? but it gets quite complicated if we try to use a heuristic once there is a parent `.gitignore`.

---

_Closed by @bensgilbert on 2025-12-21 21:21_

---
