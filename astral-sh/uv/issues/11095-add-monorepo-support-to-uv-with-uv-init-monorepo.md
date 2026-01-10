---
number: 11095
title: "Add monorepo support to uv with `uv init --monorepo` and other supporting commands"
type: issue
state: open
author: matthewadams
labels:
  - enhancement
assignees: []
created_at: 2025-01-30T14:19:05Z
updated_at: 2025-02-21T21:11:58Z
url: https://github.com/astral-sh/uv/issues/11095
synced_at: 2026-01-10T01:25:01Z
---

# Add monorepo support to uv with `uv init --monorepo` and other supporting commands

---

_Issue opened by @matthewadams on 2025-01-30 14:19_

### Summary

As mentioned in https://github.com/astral-sh/uv/issues/10960, the completion of that issue could lead to new features in uv that might include monorepo scaffolding via something like `uv init --monorepo` and even, given an existing, uv-compliant monorepo, scaffolding where a developer could add new libraries and applications to the monorepo, like:

```
mkdir -p "$MONOREPO_ROOT_DIR"
cd "$MONOREPO_ROOT_DIR"
uv init --monorepo --interactive # prompts for libraries & applications to be included, addressing public/private visibility

# perhaps adds monorepo metadata to the root pyproject.toml file or a file like monorepo.uv or uv.toml describing the projects comprising the monorepo

# ...some time later...

# thanks to uv monorepo metadata, uv is monorepo-aware

uv monorepo init --lib foobar # adds a new library project to the monorepo
# maybe supports visibility options like --public, --private, or even --unpublished
# for libraries intended to be embedded in apps
uv monorepo init --app myapp --embeds-libs foobar,snafu # adds a new app that embeds libraries foobar & snafu
```

My suspicion is that the implementation could be monorepo lipstick over `uv` workspaces.  Also, it just occurred to me that, while this is needed in a literal monorepo, the more general concept, for lack of a better term, might be "multimodule project", "multiproject", "solution", etc.  Feel free to rename.

### Example

See https://github.com/matthewadams/uv-monorepo for WIP `uv` monorepo.

---

_Label `enhancement` added by @matthewadams on 2025-01-30 14:19_

---

_Referenced in [astral-sh/uv#10960](../../astral-sh/uv/issues/10960.md) on 2025-01-30 14:20_

---

_Comment by @rsyring on 2025-02-21 18:18_

FWIW, it seems like this is expanding the scope of uv too much?

Perhaps what you need is something like mise and it's [tasks](https://mise.jdx.dev/tasks/) and you can then build up the tasks/commands you need to help manage the mono repo.  One nice benefit of that is mise tasks are hierarchical.  So each project in the monorepo can have it's own tasks but the monorepo itself can have additional tasks.  Yet they are all managed the same way and visible with `mise tasks`.  Maybe not too.  :)

---

_Comment by @matthewadams on 2025-02-21 21:11_

@rsyring Thanks for the tip on `mise`.  However, I still am not clear on how best to structure the python monorepo/multiproject, including its `pyproject.toml` settings and/or other project files & directories.  If you could fork https://github.com/matthewadams/uv-monorepo and issue a PR that demonstrates how this might work, I'd love it.

---
