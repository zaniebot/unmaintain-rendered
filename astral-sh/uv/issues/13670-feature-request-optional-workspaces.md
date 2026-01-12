```yaml
number: 13670
title: "Feature Request: Optional Workspaces"
type: issue
state: open
author: nixjdm
labels:
  - enhancement
assignees: []
created_at: 2025-05-27T02:35:40Z
updated_at: 2025-06-19T09:08:18Z
url: https://github.com/astral-sh/uv/issues/13670
synced_at: 2026-01-12T16:01:34Z
```

# Feature Request: Optional Workspaces

---

_@nixjdm_

### Summary
I would like to specify a project dependency that optionally exists locally as a workspace. If the workspace directory is missing, it gracefully falls back to however else the dependency is described in the `pyproject.toml`.

### Use Case
Consider a team with several in-house libraries, or plugins to a larger project. A team member would not necessarily ever bother pulling in the submodules or standalone repos to work on a library, but another team member might, in order to develop on that library. This feature becomes more useful the larger the team and number of libraries they manage. The `pyproject.toml` could be made to seamlessly work for all team members, whether or not they work on the libraries. "Getting started" and "digging in" to the project would both be painless.

### Implementation
This could be implemented in a few ways, but I think the most obvious is to be able to mark a workspace as optional in the `[tool.uv.workspace]` table, ala

```
[tool.uv.workspace]
optional_members = [
    ...
]
members = [
    ...
]
```
The `members` list wouldn't have to duplicate the former.

Alternatively, this could apply more broadly in `[tool.uv.sources]`, but in my opinion, this is would take more careful thinking because it could touch other types of sources. There's more going on in `sources`. Placing the change in `[tool.uv.workspace]` is the KISS approach.

Another alternative is to not make `uv` err out if a workspace is simply missing, and change nothing about the pyproject.toml. I count the current behavior as a feature though, rather than an oversight.

---

_Label `enhancement` added by @nixjdm on 2025-05-27 02:35_

---

_Comment by @pirate on 2025-06-19 09:08_

I would also find this super useful, currently it seems hard to try to install from a local path and fall back to PyPI if the workspace isnt found

---
