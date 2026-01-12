```yaml
number: 4574
title: Support private virtualenvs and locks for workspace members
type: issue
state: open
author: idlsoft
labels:
  - needs-design
  - needs-decision
assignees: []
created_at: 2024-06-26T23:53:32Z
updated_at: 2025-02-02T14:19:54Z
url: https://github.com/astral-sh/uv/issues/4574
synced_at: 2026-01-12T15:58:50Z
```

# Support private virtualenvs and locks for workspace members

---

_@idlsoft_

Workspace members currently share a single `uv.lock` and `.venv`, which comes with a couple of side-effects:
- there can't be two packages with conflicting dependencies, even if they are independent executables.
- you can accidentally reference code from any workspace member without having to declare a dependency, and unit-tests or linters are unlikely to notice.

This is unlike most languages, but usually no big deal, and having one `.venv` is convenient for most use-cases.

But it isn't for everyone, and I think it would be useful to introduce a distinct kind of member, which:
- will be excluded from the shared `uv.lock`/`.venv`
- will have its own `uv.lock`/`.venv`, with only itself and its dependencies (workspace and otherwise).

I have tried to achieve this with one simple [change](https://github.com/astral-sh/uv/pull/4499), and it mostly works, but it's clunky and I think it could be much better.

Let's say we have this tree:
```
monorepo
├── apps
│   ├── app1
│   └── app2
└── libs
    ├── lib1
    └── lib2
```
And let's say we're ok with libraries using the default shared lock, but apps should not.

The workspace could be defined as
```toml
[tool.uv.workspace]
members = ["libs/*"]
private-members = ["apps/*"]
```
This is both explicit and efficient because workspace discovery won't have to read individual `pyproject.toml` files.
Wrote [a basic implementation](https://github.com/astral-sh/uv/pull/4610) with a sample workspace.

Hope this makes sense.



---

_Comment by @mjclarke94 on 2024-06-27 00:01_

This would be a really clean solution to the refactor I'm trying to do.

It also seems like a lightweight change that unlocks a lot of the what Pants can do (finding the minimum set of dependencies required for each project, although that works per file and is a lot more powerful).

---

_Label `needs-design` added by @zanieb on 2024-06-27 02:25_

---

_Label `needs-decision` added by @zanieb on 2024-06-27 02:25_

---

_Comment by @charliermarsh on 2024-06-28 10:46_

How does this differ from excluding the projects from the workspace?

---

_Comment by @idlsoft on 2024-06-28 11:12_

> How does this differ from excluding the projects from the workspace?

Excluded projects cannot use workspace dependencies, these can.
And they're  allowed to have conflicting dependencies because they use private `uv.lock` and `.venv` directories and aren't included in `members_as_requirements`

---

_Comment by @charliermarsh on 2024-06-28 11:34_

It's an interesting idea... I understand the motivation but needs some thought / discussion before committing to it.

---

_Comment by @idlsoft on 2024-06-28 14:00_

> needs some thought / discussion before committing to it.

Yesterday I couldn't resist trying to implement it, it turned out to be a lot more straightforward than I expected.
Created https://github.com/astral-sh/uv/pull/4610 as a POC.

---

_Comment by @idlsoft on 2024-07-02 18:05_

> It's an interesting idea... I understand the motivation but needs some thought / discussion before committing to it.

Rye had a [similar issue](https://github.com/astral-sh/rye/issues/615) raised a little while ago.

---

_Comment by @pvardanis on 2024-08-14 17:17_

@idlsoft @charliermarsh that would be really nice to have as a feature, especially for us that work on monorepos. I'm working on such a monorepo where some of the members of the workspace have conflicting dependencies (that is common for ML libraries), but also some common ones. That would require having separate venvs/locks for those packages but I'd still like to have common dependencies for all of the packages on the workspace level vs. not having a workspace at all, so I don't have repeated/common dependencies specified in multiple `pyproject.toml` files.

The example repo on your PR is exactly what I was looking for: https://github.com/astral-sh/uv/tree/5f2d0323d6330395dc8b62c6a126a234176165f0/scripts/workspaces/shared-libs.

---

_Comment by @lambda-science on 2025-01-22 16:00_

Is it still not possible to have a .lock file per workspace member ?

---

_Comment by @Lenormju on 2025-02-02 01:56_

The PR [!4610 ](https://github.com/astral-sh/uv/pull/4610) by @idlsoft has not been updated since a few months, and now has merge conflicts.
I am too interested in this feature : I have a repository containing several tools, each a workspace member, all under a common lockfile, but each tool have different needs, and I don't want to install everything in each's Dockerfile, I'd prefer to have per-tool lockfiles.

---

_Comment by @MilkClouds on 2025-02-02 02:03_

Same opinion & need for me as comment above.

---

_Comment by @Lenormju on 2025-02-02 14:19_

In the Dockerfile I copied the member's `pyproject.toml` and the global `uv.lock`, then `RUN uv sync --frozen -no-dev`, and it installed only the member's dependencies (using information from the lockfile).
It's a bit cumbersome to have to add the global lockfile when building each tool's Dockerfile, but it worked for me.

---
