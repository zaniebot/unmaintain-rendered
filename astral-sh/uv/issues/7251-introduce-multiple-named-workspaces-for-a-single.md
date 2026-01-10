---
number: 7251
title: Introduce multiple, named-workspaces for a single project
type: issue
state: open
author: goraje
labels: []
assignees: []
created_at: 2024-09-10T11:00:01Z
updated_at: 2025-09-23T17:18:06Z
url: https://github.com/astral-sh/uv/issues/7251
synced_at: 2026-01-10T01:24:12Z
---

# Introduce multiple, named-workspaces for a single project

---

_Issue opened by @goraje on 2024-09-10 11:00_

**Introduction**
In our project we are dealing with the situation where we develop multiple Python packages and store them in a single monorepo. The packages that we are working on can essentially be divided into two groups (each of which could constitute a separate workspace), with some of the packages (workspace members) being common between the two groups. A simplified layout of the monorepo would look as follows.
```
.
├── package1_groupA
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── package1_groupa
│           └── __init__.py
├── package1_groupB
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── package1_groupb
│           └── __init__.py
├── package2_groupA
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── package2_groupa
│           └── __init__.py
├── package_common
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── package_common
│           └── __init__.py
└── pyproject.toml
```
**What would we like to achieve?**
We would like to define two distinct workspaces on the root level of the monorepo with the ability to create two separate lockfiles.
This can be achieved currently, however in a roundabout way by storing the workspace definitions in dedicated subdirectories since the lockfiles are going to get overwritten by executing `uv lock` even if we had two separate `uv.toml`'s and used `uv --config-file`.

**Proposal**
Introduction of named workspaces with adjustable lockfile names could  give the users the ability to customize how they want to group dependencies for different combinations of members, which would be a very useful feature for monorepo users that would facilitate using `uv` in scenarios similar to the one described above.  For example while taking into the account the layout shown in the introduction we could envision having a definition similar to:
```
# pyproject.toml

[tool.uv.workspace.group_a]
members = ["package_common", "*_groupA"]
lockfile = "uv-groupA.lock"

[tool.uv.workspace.group_b]
members = ["package_common", "*_groupB"]
lockfile = "uv-groupB.lock"
```
followed with the hypothetical ability to sync a chosen workspace eh>
```
uv sync --workspace group_a
```


---

_Comment by @brendan-morin on 2025-09-23 17:18_

I think this makes sense and seems to be a similar but  more thought out approach to my comment here: https://github.com/astral-sh/uv/issues/6356#issuecomment-2305264377.

---
