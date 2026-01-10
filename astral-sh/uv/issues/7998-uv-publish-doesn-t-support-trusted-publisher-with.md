---
number: 7998
title: "uv publish doesn't support trusted publisher with environment name"
type: issue
state: closed
author: iMicknl
labels:
  - question
assignees: []
created_at: 2024-10-08T09:24:10Z
updated_at: 2024-10-11T19:18:30Z
url: https://github.com/astral-sh/uv/issues/7998
synced_at: 2026-01-10T01:24:22Z
---

# uv publish doesn't support trusted publisher with environment name

---

_Issue opened by @iMicknl on 2024-10-08 09:24_

If we want to leverage `uv publish` in GitHub Actions, best is to set a trusted publisher as mentioned in the uv docs. However, it seems there is no possibility to set the environment name via `uv publish`. Setting the environment name is a best practice according to the PyPi docs.

> Configuring an environment is optional, but strongly recommended: with a GitHub environment, you can apply additional restrictions to your trusted workflow, such as requiring manual approval on each run by a trusted subset of repository maintainers.
https://docs.pypi.org/trusted-publishers/adding-a-publisher/

uv version: 0.4.18



---

_Assigned to @konstin by @zanieb on 2024-10-08 22:46_

---

_Comment by @konstin on 2024-10-09 09:28_

The environment name is set in the GitHub Action configuration (https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/using-environments-for-deployment) and on PyPI; `uv publish` (or twine) are not involved with the environment setting.

---

_Label `question` added by @konstin on 2024-10-09 09:28_

---

_Comment by @iMicknl on 2024-10-11 19:18_

Thanks, @konstin. Not sure how I missed that, since I did set it up the same way for my Poetry project... 

---

_Closed by @iMicknl on 2024-10-11 19:18_

---
