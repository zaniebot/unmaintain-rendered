```yaml
number: 7018
title: "Feature request: lockfile import"
type: issue
state: open
author: Kobzol
labels:
  - wish
assignees: []
created_at: 2024-09-04T13:38:40Z
updated_at: 2024-09-04T14:26:33Z
url: https://github.com/astral-sh/uv/issues/7018
synced_at: 2026-01-10T04:45:10Z
```

# Feature request: lockfile import

---

_Issue opened by @Kobzol on 2024-09-04 13:38_

Hi, thank you for an awesome package management tool! :) I have a suggestion for a feature that I would appreciate very much when porting existing projects to `uv`.

Dependencies of some existing Python projects are not managed very well, and they only exist in the form of a `requirements.txt` lockfile, or even just a set of installed packages in some virtual environment (from which you can generate the lockfile with `pip freeze`). Migrating such a project to `uv` is currently a bit annoying. The naive approach is to take all the dependency specifications in the lockfile and put them inside the `dependencies` array in `pyproject.toml`, but that's usually not what you want, because it also contains transitive dependencies.

What would help automate this process is if there was a `uv` command that would read an existing (`pip`) lockfile, transform the packages into a graph, calculate a topological sort, and then separate the packages that are not depended upon by any other package in the lockfile (these form the "root" package dependencies with a high probability). It could then import these into the `dependencies` array, and potentially interactively ask the user what to do about the rest of the packages (but even just separating the root packages would be quite useful).

Why do I need to do this? Well, if I have an existing project with a set of packages, I want to make sure that after I migrate to `uv`, I will essentially have the same lockfile, to avoid any surprises. If I just took the known root level packages and migrated them to `dependencies`, the versions of the transitive dependencies would most likely be updated by `uv` (especially since there's currently no support for changing versions of transitive dependencies in a lockfile, AFAIK).

Let me know if it makes sense :)

---

_Comment by @zanieb on 2024-09-04 14:26_

@Kobzol this is roughly supported with `uv add -r requirements.in` or `uv add -r requirements.txt`.

I don't think we'll try to infer which packages are actual direct dependencies in the near future — it seems hard to get right and I'm not sure what the interface would look like. It does sound cool and helpful though — if someone were to prototype it, I'd take a look.

---

_Label `wish` added by @zanieb on 2024-09-04 14:26_

---
