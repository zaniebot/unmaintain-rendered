```yaml
number: 8585
title: Bumping minimum version of a dependency
type: issue
state: open
author: tjni
labels:
  - enhancement
assignees: []
created_at: 2024-10-26T04:14:54Z
updated_at: 2024-10-28T20:46:27Z
url: https://github.com/astral-sh/uv/issues/8585
synced_at: 2026-01-12T15:59:30Z
```

# Bumping minimum version of a dependency

---

_@tjni_

I have a use case to provide some automation to update a package A to version X across many repositories. Some goals:

1. If a package B depends on A in its lock file, and A is at version < X, then (try to) update A to exactly X.
2. If a package B depends on A in its lock file, and A is at version >= X, then leave it alone.
3. If a package B specifies a direct dependency on A, and the specification allows versions < X, then update the specification to disallow those versions.

This approach lets us gradually increase dependency freshness: often it's easier to bump versions in steps instead of to the latest. It also helps with migrations.

I also have not thought through all of the nuances of doing this. So I'm interested to know:

1. Is this possible?
2. Is this even a good idea?
3. Are there subtleties I'm missing?
4. Are there alternatives?


---

_Comment by @zanieb on 2024-10-26 16:22_

I think there are some similar requests for this, but https://github.com/astral-sh/uv/issues/6794 is the only one I can find at the moment.

Generally yeah this would be nice to have. It sounds hard though :)

---

_Label `enhancement` added by @zanieb on 2024-10-26 16:22_

---

_Comment by @tjni on 2024-10-28 20:46_

Thank you for your response and the link!

I gave this a little more thought, and I think I can break my request down into a couple of subproblems.

1. I want to update direct dependency (or dependency group) constraints in `pyproject.toml`. This is what https://github.com/astral-sh/uv/issues/6794 addresses. In the meantime, I can do my own TOML parsing and writing. This is what we do to handle poetry too.
2. I want to shape the result of the constraint resolver during lock file generation through adding additional constraints. This is possible already through `constraint-dependencies` and `override-dependencies`, but I don't want this in the `uv.lock` file (or, at least, I don't want it to cause `uv sync --locked` to fail without an updated `pyproject.toml`).

How I want to shape the constraint is conditional on the current version of package A. I can control that by manually parsing the current versions in `uv.lock` and invoking (2) as appropriate.

Or a slightly more out there idea, consider a request like https://github.com/astral-sh/uv/issues/5492, it can be expressed (in theory) at a dependency level as `A==lowest` with the default being `A==highest` if we can extend the constraint language. Then, what I'm asking for is an additional extension, which is `A>=current_lockfile_version`, after which I can describe my ask with three constraints:

1. A>=X
2. A>=current_lockfile_version
3. A==lowest

Just some brainstorming and a couple of scattered thoughts.

---
