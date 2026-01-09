---
number: 7994
title: Updating pyproject.toml version during build results in incorrect version in uv.lock
type: issue
state: closed
author: david-waterworth
labels: []
assignees: []
created_at: 2024-10-08T04:12:24Z
updated_at: 2025-06-15T11:15:13Z
url: https://github.com/astral-sh/uv/issues/7994
synced_at: 2026-01-07T13:12:17-06:00
---

# Updating pyproject.toml version during build results in incorrect version in uv.lock

---

_Issue opened by @david-waterworth on 2024-10-08 04:12_

This is more an observation rather than an issue.

I'm using a ci tool that parses commit history, generates the "next" version and updates the version in pyproject.toml (the tool is `python-semantic-release`).

What I just noticed was, since `python-semantic-release` updates `pyproject.toml` when it performs a build, once the build has finished `uv.lock` contains the "wrong" version for the project, i.e. 

```
[[package]]
name = "my-package"
version = "0.2.1.dev0"
source = { virtual = "." }
```

Since  `python-semantic-release`  bumped the version to "0.2.1". To fix I added `uv sync && build.sh && git add uv.lock` - this updates the lock file and stages it (with the other files `python-semantic-release`  modifies.

I'm not sure if this causes more problems than it fixes, but I thought I'd mention it - I think in general it's probably a better practice to generate tags based on the pyproject.toml->project.version rather than the other way around?


---

_Comment by @zanieb on 2024-10-08 04:28_

Thanks for the report.

I think @charliermarsh has some context on this.

---

_Assigned to @charliermarsh by @zanieb on 2024-10-08 04:28_

---

_Comment by @my1e5 on 2024-10-08 09:29_

Related: https://github.com/astral-sh/uv/issues/7533

---

_Comment by @david-waterworth on 2024-10-08 22:16_

@my1e5 yeah that's the same - shall I close this?

---

_Referenced in [astral-sh/uv#7533](../../astral-sh/uv/issues/7533.md) on 2024-10-08 22:22_

---

_Comment by @my1e5 on 2024-11-19 16:33_

All discussion is being moved to #7533. So this can be closed.

---

_Closed by @charliermarsh on 2024-11-19 16:34_

---

_Comment by @aqib-bhat on 2025-06-15 11:15_

I was also experiencing the `uv.lock` going out of sync when `python-semantic-release` would bump the version in `pyproject.toml`. I fixed this by:
- Adding a step after the semantic release that updates the version of my project in the `uv.lock` file by running: `uv lock --upgrade-package <project_name>` followed by committing and pushing the change.
  - Link to my GitHub workflow file: https://github.com/aqib-oss/sonar-qube-gh-action/blob/main/.github/workflows/main-workflow.yaml#L74
  - Thankfully, this does not trigger another run of the `main` branch workflow, however, if it did, it can easily be handled by adding a pre-check job that determines if the last commit was for syncing the project version in the `uv.lock` file, and the main job will run only if the output from the pre-check job is `true`.

---
