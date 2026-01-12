```yaml
number: 12547
title: Support specifying path to project via CLI and env var
type: issue
state: closed
author: AgentK9
labels:
  - enhancement
assignees: []
created_at: 2025-03-29T19:21:05Z
updated_at: 2025-03-30T14:59:45Z
url: https://github.com/astral-sh/uv/issues/12547
synced_at: 2026-01-12T16:01:06Z
```

# Support specifying path to project via CLI and env var

---

_@AgentK9_

### Summary

I would like to be able to interact with uv with a project installed in a specific folder.

In particular, I'm trying to programmatically work with uv, so the only way to change environments is to use `os.chdir`.
However, this would also be helpful in CI environments, or with dealing with subprojects in a monorepo.

### Example

`uv init abc`

`UV_PROJECT_PATH="abc" uv add pydantic`
OR
`uv add --project-path abc pydantic`

---

_Label `enhancement` added by @AgentK9 on 2025-03-29 19:21_

---

_Comment by @charliermarsh on 2025-03-29 20:05_

Are you looking for `--project`?

---

_Comment by @AgentK9 on 2025-03-29 20:19_

Yes - I was looking in the documentation for the environment variables and in the "concepts" section under "projects" - this was not described in either place (which makes sense in hindsight, haha). Feel free to close this - probably a duplicate.

I could also rewrite/reopen to make this for an issue for an environment variable for the `--project` option.

---

_Comment by @charliermarsh on 2025-03-30 14:59_

I think I'd suggest taking a look at the CLI help! Like `uv add --help`.

---

_Closed by @charliermarsh on 2025-03-30 14:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-30 14:59_

---
