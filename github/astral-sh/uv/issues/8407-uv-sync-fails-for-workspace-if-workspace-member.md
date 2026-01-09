---
number: 8407
title: uv sync fails for workspace if workspace member is declared to come from a named index
type: issue
state: open
author: thorbenk
labels:
  - question
assignees: []
created_at: 2024-10-21T07:37:17Z
updated_at: 2024-10-22T19:33:00Z
url: https://github.com/astral-sh/uv/issues/8407
synced_at: 2026-01-07T13:12:17-06:00
---

# uv sync fails for workspace if workspace member is declared to come from a named index

---

_Issue opened by @thorbenk on 2024-10-21 07:37_

I ran into an issue with the new named indexes feature and workspaces.

Here is a minimal reproducible example (I'm on `uv 0.4.24`)

- Start with the `albatross-virtual-workspace` example (https://github.com/thorbenk/uv-workspaces-question/commit/af0e74139f464c625dfe779cbdef0ebdebd8fb81)
- Move the source declarations to the root (https://github.com/thorbenk/uv-workspaces-question/commit/48aad9e8ddd0c3ca5fb723ecf37dd77b3a74b8dc, as discussed in https://github.com/astral-sh/uv/issues/6786 this is OK and indeed, does NOT break `uv sync`)
- But declaring `seeds` to come from a named index _breaks_ `uv sync` (https://github.com/thorbenk/uv-workspaces-question/commit/869f602b75f759a1da3a8c8ee9d8b5c9777fc57b)
  - (It's probably not important if that package actually exists on that index; in my original private repository that was the case)

The error message is:

```
error: Failed to generate package metadata for `bird-feeder==1.0.0 @ editable+packages/bird-feeder`
  Caused by: Failed to parse entry for: `seeds`
  Caused by: Package is not included as workspace package in `tool.uv.workspace`
```

---

_Assigned to @charliermarsh by @zanieb on 2024-10-21 13:33_

---

_Comment by @charliermarsh on 2024-10-21 14:55_

So you have a workspace member named `seeds`, but you're trying to define an alternate source for it? I don't think this is "allowed" right now: workspace members have to come from workspace sources.

(@konstin -- is that right?)

---

_Comment by @charliermarsh on 2024-10-21 15:00_

I actually think this isn't possible to support because we lock the full workspace together, so we'd _always_ be trying to resolve the workspace member and the from-the-index member at the same time, which can't succeed.

---

_Label `question` added by @charliermarsh on 2024-10-21 15:00_

---

_Comment by @thorbenk on 2024-10-21 15:28_

OK, it looks like I may have been misusing this behavior for my use-case then.

The following setup worked for me before I tried to switch to named indexes:

- The workspace members are independent python projects, each with it's own git repository, `uv.lock` file and associated CI pipeline. They depend on each other through released package versions in their `pyproject.toml` files.
- These packages are all published to a private index.
- For development purposes, I have created a "meta" repository to tie multiple of these packages together. The workspace members are git submodules.
When hacking on a feature that spans multiple of these packages, I found the workspace setup very useful: I have _overwritten_ the sources in the meta repository to declare the packages as workspace members, even though they themselves still reference each other through released package versions on some index.
Everything is installed as editable packages inside a single workspace and it is easy to test that big new feature. Once everything works, I can still publish each workspace member separately as its own independent project.
- Only after I tried porting to the new named indexes, I ran into the above problems

I guess I was expecting (hoping?) that the declaration of `seeds` as a workspace source would override if I had `"seeds==2.0"` in `bird-feeder`, allowing me to hack with local editable sources.

Probably I'm missing something, surely there has to be a better way to support such a development workflow?
(when I was using `poetry` before I used to constantly switch between path dependencies and released package versions manually in the `pyproject.toml`, which was a really bad way of doing things).



---

_Comment by @konstin on 2024-10-22 10:57_

Editing dependencies in remote repositories is one of those problems we want to build a dedicated workflow for, but don't have good support yet.

Workspaces aren't well suited for this: A workspace assumes that it's the only source of truth for all packages it includes, so if the root says all of these packages are local path dependencies, the members can't say they are index dependencies. As a rule of thumb, if a member has it's own lockfile, it's probably not suited as a workspace member.

For now, using the submodules as editable path dependencies instead of as workspace members is likely the better approach.

---

_Comment by @thorbenk on 2024-10-22 19:23_

@konstin Thanks for your explanation. I found the documentation in that regard a bit confusing (https://docs.astral.sh/uv/concepts/dependencies/#path), as it explicitly advises

> However, it is recommended to use [workspaces](https://docs.astral.sh/uv/concepts/workspaces/) instead of manual path dependencies.

So, is the following setup something you would recommend doing: https://github.com/thorbenk/uv-path-dependencies-test/tree/main (it's a minimal example)?

---

_Comment by @konstin on 2024-10-22 19:32_

Yeah we can improve the wording: https://github.com/astral-sh/uv/pull/8478

Submodules are an odd case because they are kinda part of the repository and kinda not; overall i hope to properly solve your use case with a dedicated workflow.

---
