```yaml
number: 2266
title: pip compile constraining combined with git based dependencies
type: issue
state: closed
author: gwdekker
labels:
  - bug
assignees: []
created_at: 2024-03-07T07:14:40Z
updated_at: 2024-03-07T23:47:32Z
url: https://github.com/astral-sh/uv/issues/2266
synced_at: 2026-01-12T15:58:36Z
```

# pip compile constraining combined with git based dependencies

---

_@gwdekker_

First things first: I have to say that I never felt so encouraged to file issues at an open source project - as it is obvious that the chance of getting it fixed and soon is orders of magnitude higher than anywhere else! 

Use case that fails:
- I want to have requirements and dev-requirements that are guaranteed to be compatible: I want to make sure I am not running a package version in production that I do not have in my dev environment.
- The suggested workflow for this I found is to compile the dev requirements first, and then use the resulting dev-requirements.txt as a constraint file for solving the production requirements like so:
```
	uv pip compile --extra dev -o requirements-dev.txt pyproject.toml 
	uv pip compile --constraint requirements-dev.txt -o requirements.txt pyproject.toml 
```

This fails in the case of having packages installed directly from a git repo. I have a repo in pyproject.toml like this:

`PACKAGE_NAME @ git+https://__token__:${GITLAB_ACCESS_TOKEN}@gitlab.com/PATH_TO_REPO.git@my_branch`

What happens is that the branch name gets resolved to a commit hash, and as a result the resolving fails:

```
error: Requirements contain conflicting URLs for package `PACKAGE_NAME`:
- git+https://__token__:${GITLAB_ACCESS_TOKEN}@gitlab.com/PATH_TO_REPO.git@intraday-main
- git+https://__token__:${GITLAB_ACCESS_TOKEN}@gitlab.com/PATH_TO_REPO@THE_FULL_COMMIT_SHA

```




---

_Comment by @charliermarsh on 2024-03-07 14:17_

I think we can support this as a sort of special-case (when URLs conflict, and one is a pinned VCS SHA, accept the pinned VCS SHA).

---

_Label `bug` added by @charliermarsh on 2024-03-07 14:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-07 14:17_

---

_Comment by @charliermarsh on 2024-03-07 14:17_

I'm just gonna categorize it as a bug since that's what users would expect.

---

_Comment by @gwdekker on 2024-03-07 20:15_

Thanks for sharing my comment on X, happy to see you appreciated it! Keep up the amazing work :)

---

_Closed by @charliermarsh on 2024-03-07 23:47_

---
