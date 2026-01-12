```yaml
number: 15615
title: Add support for remote pyproject.toml files
type: pull_request
state: closed
author: yumeminami
labels: []
assignees: []
base: main
head: feat/remote-pyproject-toml-support
created_at: 2025-09-01T04:07:46Z
updated_at: 2025-09-09T05:54:25Z
url: https://github.com/astral-sh/uv/pull/15615
synced_at: 2026-01-12T16:11:51Z
```

# Add support for remote pyproject.toml files

---

_@yumeminami_

### Summary

Implements support for installing dependencies from remote pyproject.toml files via HTTP/HTTPS URLs, resolving GitHub issue #15508.

### Test

Write a test case using the remote URL from #15508.

---

_Comment by @konstin on 2025-09-07 15:26_

Hi, and thanks for contributing to uv!

There's some extra complexity here for us to determine what should be part of this feature. For example, `uv pip install -r https://.../requirements.txt` supports having `-e ..` in that remote file and will use the local parent directory. We support it because `pip install -r https://.../requirements.txt` does the same, otherwise we likely wouldn't allow this (a remote file shouldn't be able to browse the local file system). For `uv pip install -r https://.../pyproject.toml` it's unclear how many pyproject.toml features we want or should support. It's also unclear if pip will support this in the future and if we need to ensure we will be compatible with pip (or pip with uv, which would cause a different form of problem). For example, if you had `path = ".."` in `tool.uv.sources`, should we install from the local parent directory, should we look in the parent path in the URL of the `pyproject.toml` or should we error? What about other features such as workspace and dependency groups? We currently have only a single use case for this in https://github.com/astral-sh/uv/issues/15508, which makes it hard to design something, we're missing sufficient motivation to design around.

We do think it makes sense to support remote `pyproject.toml` similar to remote `requirements.txt` in general, and we appreciate PRs to uv, but I'm afraid we currently have a hard time reviewing and merging this PR due to difficulties above.

---

_Comment by @yumeminami on 2025-09-09 05:54_

@konstin thank your feedback. i will close it.

---

_Closed by @yumeminami on 2025-09-09 05:54_

---
