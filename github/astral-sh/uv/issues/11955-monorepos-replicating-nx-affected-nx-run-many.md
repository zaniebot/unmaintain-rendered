---
number: 11955
title: "Monorepos:  Replicating `nx affected`, `nx run-many` with `uv`."
type: issue
state: open
author: gregglind
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-03-04T16:08:54Z
updated_at: 2025-10-25T09:29:41Z
url: https://github.com/astral-sh/uv/issues/11955
synced_at: 2026-01-07T13:12:18-06:00
---

# Monorepos:  Replicating `nx affected`, `nx run-many` with `uv`.

---

_Issue opened by @gregglind on 2025-03-04 16:08_

### Summary

[`nx affected`](https://nx.dev/ci/features/affected) promises to "run only tasks affected by a PR".  

Having `uv tree` and `uv lock --check` makes me wonder if uv could replace this command?

If this is out of scope for `uv` proper, is there a suggested better tool or pattern?

### Context

Non-workspace monorepos, where each project is pytest testable, but there is a root pyproject to oversee  and enforce code patterns (ruff, etc), python versions, and other shared contraints.




### Example

_No response_

---

_Label `enhancement` added by @gregglind on 2025-03-04 16:08_

---

_Comment by @zanieb on 2025-03-04 17:30_

We're interested in this, but we're far out from implementing it in a robust way.

The most relevant work here is https://github.com/astral-sh/ruff/pull/13402 which is used by some people for test selection.

---

_Label `wish` added by @zanieb on 2025-03-04 17:30_

---

_Comment by @gregglind on 2025-03-04 17:46_

Thanks for sharing that link and validating the desire.

Even "non-robust" ways (packages with source lines in the lockfile, assuming they are all in projects with a `pyproject.toml`) are a great starting point!



---

_Comment by @gregglind on 2025-03-04 22:20_

Further amplification:  a place where UV can add value (the "most missing piece") is by exposing more of the dependency graph, either as part of `lock` or `tree`.    This is orthogonal to the discussion in https://github.com/astral-sh/uv/issues/5903.  

Building a `run-many` task runner that uses that information might be a simple as calling `uv run --project` or `uv run --directory` for each line in that graph (bottom-up).

In particular, listing every pyproject.toml that would be involved in a particular uv lock scenario would be very helpful, and strictly better than `grep`.  

`uv -v lock --dry-run |& grep static` has lines like

```
DEBUG Found static `pyproject.toml` for: my-lib @ file:///Users/glind/some/monorepo/path`



---

_Comment by @lpetre on 2025-03-19 12:11_

I've written a github action that compares to uv.lock files using `uv tree --frozen --no-depupe` on the current and base revision, plus the changed files (using [Ana06/get-changed-files](https://github.com/Ana06/get-changed-files)).

The output of the action is a list of workspace actions that have been effected by the PR. This handles intra workspace dependencies. It runs in about 8 seconds in my monorepo of about ~35 python packages

---

_Comment by @Spenhouet on 2025-10-25 09:29_

Maybe this helps: https://github.com/astral-sh/uv/issues/6356#issuecomment-2983217371

---
