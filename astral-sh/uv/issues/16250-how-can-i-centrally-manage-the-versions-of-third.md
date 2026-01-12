```yaml
number: 16250
title: How can I centrally manage the versions of third-party libraries?
type: issue
state: closed
author: ya7010
labels:
  - question
assignees: []
created_at: 2025-10-11T06:56:20Z
updated_at: 2025-10-21T00:56:56Z
url: https://github.com/astral-sh/uv/issues/16250
synced_at: 2026-01-12T16:02:27Z
```

# How can I centrally manage the versions of third-party libraries?

---

_@ya7010_

### Question

When developing in a monorepo with Rust's Cargo, there is a way to unify the versions of the libraries you use.

Workspace `Cargo.toml`

```toml
[workspace.dependencies]
serde = "1.0"
```

Member `Cargo.toml`

```toml
[dependencies]
serde.workspace = true
```

In `uv`, first-party libraries can be managed using `tool.uv.workspace.members` and `tool.uv.sources`. Is there an official recommendation for managing third-party libraries?

I am currently using it in the following approach.

Workspace `pyproject.toml`

```toml
[package]
name = "my-project"
dependencies = [
  "pydantic>=2.10,<3.0", # Specify the version only in the workspace.
]

[tool.uv.workspace]
members = [
  "members/*",
]
```

Member `Cargo.toml`

```toml
[project]
name = "my-member1"
dependencies = [
  "pydantic" # Version is not specified for each member.
]

[tool.uv.sources]
my-member2 = { workspace = true }
```

Is this approach correct? Do you have any better suggestions?

I couldn't find it in the documentation, so I want to confirm if the policy is correct.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @ya7010 on 2025-10-11 06:56_

---

_Comment by @konstin on 2025-10-20 12:32_

There is no really good approach for this, as PEP 621 effectively bans copying Cargo, but your approach looks like a good, functional workaround.

---

_Comment by @ya7010 on 2025-10-21 00:56_

Thank you!

At least for the time being, I'll continue using this method I've been using.

---

_Closed by @ya7010 on 2025-10-21 00:56_

---
