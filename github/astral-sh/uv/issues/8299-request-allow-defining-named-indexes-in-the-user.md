---
number: 8299
title: "[Request] Allow defining named indexes in the user-level configuration file"
type: issue
state: open
author: jonasdeyson
labels:
  - needs-decision
assignees: []
created_at: 2024-10-17T17:53:21Z
updated_at: 2024-10-24T14:51:26Z
url: https://github.com/astral-sh/uv/issues/8299
synced_at: 2026-01-07T13:12:17-06:00
---

# [Request] Allow defining named indexes in the user-level configuration file

---

_Issue opened by @jonasdeyson on 2024-10-17 17:53_

It would be nice to be able to define named private indexes (including the credentials) in the user-level configuration file, and use them in a project.

Ex:

In the user-level config file (`~/.config/uv/uv.toml`):

```toml
[[index]] 
name = "myindex" 
url = "https://__token__:<token>@<myindex domain>/simple" 
explicit = true
```

In a project's `pyproject.toml` file:

```toml
[tool.uv.sources]
mypackage = { index = "myindex" }
```

Currently, if I try this it gives the error:

```text
error: Failed to generate package metadata for `proj==0.1.0 @ virtual+.`
  Caused by: Failed to parse entry for: `mypackage`
  Caused by: Package `mypackage` references an undeclared index: `myindex`
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `needs-decision` added by @charliermarsh on 2024-10-17 18:03_

---

_Comment by @charliermarsh on 2024-10-18 12:59_

I can understand why it would be helpful, but just a note that this was an intentional decision in the implementation -- we didn't want to allow references to global indexes that don't exist within the project and so may not exist on other machines.

---

_Comment by @lucas-labs on 2024-10-23 20:59_

We're facing a similar situation. We use an internal gitea repository + package index for some of our internal libraries.

If we want to use keyring for credentials, we still need set the username in the url.
Each developer has their own username and password, so we can't commit the `pyproject.toml` like this:

```toml
# pyproject.toml

[tool.uv.sources]
lib1 = { index = "private-repo" }

[[index]]
name = "private-repo"
url = "http://my.own.username@host/api/packages/org/pypi/simple/"
explicit = true
```

We ended up creating a shared user with read privileges, but that's suboptimal imo. 

Is there a workaround we haven't think of?

Cheers!


---

_Comment by @charliermarsh on 2024-10-23 23:39_

@lucas-labs -- You could set `UV_INDEX_PRIVATE_REPO_USERNAME=` to whatever each user's username is (and omit the credentials from the `pyproject.toml`. Does that work?

See: https://docs.astral.sh/uv/configuration/indexes/#providing-credentials

---

_Comment by @lucas-labs on 2024-10-24 14:51_

@charliermarsh YES! that worked. I don't know how I missed that one! Thanks!

---

_Referenced in [astral-sh/uv#9554](../../astral-sh/uv/issues/9554.md) on 2024-12-01 14:49_

---
