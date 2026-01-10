```yaml
number: 10894
title: "docs(git): add subdirectory example"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/add-subdirectory-example
created_at: 2025-01-23T12:29:27Z
updated_at: 2025-01-23T14:13:56Z
url: https://github.com/astral-sh/uv/pull/10894
synced_at: 2026-01-10T11:45:17Z
```

# docs(git): add subdirectory example

---

_Pull request opened by @mkniewallner on 2025-01-23 12:29_

## Summary

Closes #10889.

As kinda hinted in the issue, would adding a `--subdirectory` flag to `uv add` make sense? This would IMO be more discoverable.

## Test Plan

Ran the command added to the documentation:

```console
$ uv add git+https://github.com/langchain-ai/langchain#subdirectory=libs/langchain
Resolved 40 packages in 203ms
   Built langchain @ git+https://github.com/langchain-ai/langchain@f2ea62f63209130bfc56b1fe7d0fa7c299bbf352#subdirectory=libs/langchain
Prepared 19 packages in 837ms
Installed 37 packages in 14ms
[...]
```

---

_@charliermarsh approved on 2025-01-23 13:58_

Nice, thanks.

---

_Merged by @charliermarsh on 2025-01-23 13:58_

---

_Closed by @charliermarsh on 2025-01-23 13:58_

---

_Label `documentation` added by @charliermarsh on 2025-01-23 13:58_

---

_Branch deleted on 2025-01-23 13:59_

---

_Comment by @mickvangelderen on 2025-01-23 14:09_

> As kinda hinted in the issue, would adding a `--subdirectory` flag to `uv add` make sense? This would IMO be more discoverable.

@mkniewallner You will have to refactor how unnamed package name resolution works. Currently the resolution works by checking out an URL which accepts this `#subdirectory=<subdirectory>` fragment.

The code here https://github.com/astral-sh/uv/commit/8a6fac04f55dcafacf684916aa4b972bc145d2fb does the easy part, passing around a subdirectory parameter, but you also need to figure out how to change this https://github.com/astral-sh/uv/blob/e3a09888abaa2cd504e7511b1bd38c1f4efc9c56/crates/uv/src/commands/project/add.rs#L256-L357. 

---
