```yaml
number: 2671
title: Allow prereleases, locals, and URLs in non-editable path requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pre-local
created_at: 2024-03-26T16:03:47Z
updated_at: 2024-03-27T22:17:10Z
url: https://github.com/astral-sh/uv/pull/2671
synced_at: 2026-01-12T16:05:09Z
```

# Allow prereleases, locals, and URLs in non-editable path requirements

---

_@charliermarsh_

## Summary

This PR enables the resolver to "accept" URLs, prereleases, and local version specifiers for direct dependencies of path dependencies. As a result, `uv pip install .` and `uv pip install -e .` now behave identically, in that neither has a restriction on URL dependencies and the like.

Closes https://github.com/astral-sh/uv/issues/2643.
Closes https://github.com/astral-sh/uv/issues/1853.


---

_Label `enhancement` added by @charliermarsh on 2024-03-26 16:03_

---

_Marked ready for review by @charliermarsh on 2024-03-26 18:00_

---

_Comment by @charliermarsh on 2024-03-26 18:01_

We could in theory use the same technique to resolve these recursively, upfront, for all direct URL distributions...

---

_Review requested from @zanieb by @zanieb on 2024-03-26 18:06_

---

_@charliermarsh reviewed on 2024-03-26 18:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/locals.rs`:43 on 2024-03-26 18:09_

Going to DRY this up in a separate PR (it's already repeated excessively on `main`).

---

_Comment by @charliermarsh on 2024-03-26 18:17_

A slightly different way to solve this would be to pass these paths to `SourceTreeResolver`, and then include the requirements as first-party requirements _directly_. This would be ok but I was worried about how it might impact error messages.

---

_@charliermarsh reviewed on 2024-03-26 18:32_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:23 on 2024-03-26 18:32_

The alternative solution would be to pass any source trees to `SourceTreeResolver` (so, e.g., if the user does `foo @ ./bar/baz`, we'd pass `./bar/baz` to `SourceTreeResolver` alongside any `pyproject.toml` or `setup.py` files that were passed in). Then, we'd treat the requirements as part of the `requirements` input to the resolution.

That approach would require some refactoring of `SourceTreeResolver` (since it uses `ExtrasSpecification`, which isn't a generic concept for requirements). It would also mean that the actual `Resolver` wouldn't be able to distinguish between these "lookahead" requirements and actual first-party requirements, and there may not be any indication in the error messages that they came from the local directory requirement, but that might be ok. I'm open to either.


---

_@charliermarsh reviewed on 2024-03-26 18:32_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:23 on 2024-03-26 18:32_

\cc @zanieb 

---

_Comment by @charliermarsh on 2024-03-26 22:28_

Also, I think this should be recursive… so that local deps can define local deps at any level.

---

_Comment by @charliermarsh on 2024-03-26 22:35_

Thinking about it... we could actually extend this approach to enable support for transitive URL dependencies (as long as we don't allow URL dependencies to come from registry dependencies, which is fine)... The basic idea would be: recursively resolve all direct URL references, then the requirements of all direct URL references, etc. So we'd collect all possible direct URL references upfront.

---

_Review requested from @konstin by @charliermarsh on 2024-03-27 03:49_

---

_Review comment by @konstin on `crates/uv-requirements/src/lookahead.rs`:57 on 2024-03-27 11:49_

Do we want to put that value in a global constant?

---

_@konstin approved on 2024-03-27 11:54_

---

_Comment by @charliermarsh on 2024-03-27 16:11_

I wonder if we should start by allowing this to be recursive for _local_ requirements (for editables too).

---

_Comment by @charliermarsh on 2024-03-27 16:12_

(But it's fine to merge as-is. That would be an extension of this work.)

---

_Review comment by @zanieb on `crates/uv-requirements/src/lookahead.rs`:23 on 2024-03-27 20:51_

This seems okay, but the whole `SourceTree` and `SourceTreeResolver` concept is so new that I don't have strong feelings.

I like the idea of them being included in the resolver manifest.

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_install.rs`:525 on 2024-03-27 20:53_

This is going to conflict with #2596 which refactors this already.

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:6299 on 2024-03-27 20:55_

Can you use a child of the context temporary directory instead? per #2678 

You won't need custom filters then.

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:6358 on 2024-03-27 20:55_

Same comment regarding temporary directories and filters.

---

_Review comment by @zanieb on `crates/distribution-types/src/requirements.rs`:10 on 2024-03-27 20:57_

Why here instead of `uv-requirements` or `uv-types`?

---

_@zanieb approved on 2024-03-27 20:57_

I do have some requested changes, but overall this makes sense to me.

---

_@charliermarsh reviewed on 2024-03-27 21:42_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/requirements.rs`:10 on 2024-03-27 21:42_

`uv-types` could make sense. `uv-requirements` is a heavy dependency for (e.g.) the resolver to pull in.

---

_@charliermarsh reviewed on 2024-03-27 22:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_install.rs`:525 on 2024-03-27 22:01_

Reverting since it's not required for the PR.

---

_Merged by @charliermarsh on 2024-03-27 22:17_

---

_Closed by @charliermarsh on 2024-03-27 22:17_

---

_Branch deleted on 2024-03-27 22:17_

---
