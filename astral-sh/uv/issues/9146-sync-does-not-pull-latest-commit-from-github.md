---
number: 9146
title: sync does not pull latest commit from github dependency
type: issue
state: closed
author: fccoelho
labels:
  - question
assignees: []
created_at: 2024-11-15T12:32:57Z
updated_at: 2024-11-18T19:28:24Z
url: https://github.com/astral-sh/uv/issues/9146
synced_at: 2026-01-10T01:24:37Z
---

# sync does not pull latest commit from github dependency

---

_Issue opened by @fccoelho on 2024-11-15 12:32_

I have a dependency on one of my projects, which is based on GitHub.

When I used Poetry, the `poetry update` command would always pull the latest commit in the repo of the dependency. 
That allowed my project to be in sync with the very latest changes in the main branch of the dependency.

`uv sync` does not do that by default. apparently it pulls the latest release or tag, I am not sure.

Is there some flag or option to change this default behavior to match that of poetry?


---

_Comment by @FishAlchemist on 2024-11-15 13:51_

``uv sync``: Syncing ensures that all project dependencies are installed and up-to-date with the lockfile.
Therefore, you need to relock your dependencies. You can do this using ``uv lock --upgrade``. If you want to specify a package to upgrade, use ``--upgrade-package <package>``
https://docs.astral.sh/uv/concepts/projects/#upgrading-locked-package-versions


---

_Comment by @charliermarsh on 2024-11-15 13:59_

Yeah you want `uv lock --upgrade` or `uv lock --upgrade package <package>`, as @FishAlchemist said.

---

_Label `question` added by @charliermarsh on 2024-11-15 13:59_

---

_Comment by @fccoelho on 2024-11-15 16:45_

Thanks, for the suggestion to use the `--upgrade` option. 

I haven't tried it yet because meanwhile I have found another solution to this issue: simply adding a `branch="main"` to the dependency on pyproject.toml, like so:
```toml
[tool.uv.sources]
mydep = { git = "https://github.com/author/mydep", branch = "main" }
```
Makes `uv sync` always sync to the latest commit in the branch. 

---

_Comment by @charliermarsh on 2024-11-15 16:48_

I'm not sure that that's true. Is it possible that it just worked on first `uv sync`, when you added it?

---

_Comment by @fccoelho on 2024-11-15 17:12_

> I'm not sure that that's true. Is it possible that it just worked on first `uv sync`, when you added it?

Yes, you're right. It worked before because I had also created a new tag in the dependency repo. So `uv sync` automatically upgraded to it. Now I tested without adding a new tag, after a new commit, and it didn't upgrade. 

The `uv lock --upgrade-package <package name>` worked as expected, though.  

---

_Comment by @charliermarsh on 2024-11-18 19:28_

I think this _is_ working as expected then.

---

_Closed by @charliermarsh on 2024-11-18 19:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-18 19:28_

---
