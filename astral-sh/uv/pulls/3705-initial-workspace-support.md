```yaml
number: 3705
title: Initial workspace support
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/workspaces
created_at: 2024-05-21T17:12:07Z
updated_at: 2024-05-28T07:41:56Z
url: https://github.com/astral-sh/uv/pull/3705
synced_at: 2026-01-12T16:05:48Z
```

# Initial workspace support

---

_@konstin_

Add workspace support when using `-r <path>/pyproject.toml` or `-e <path>` in the pip interface. It is limited to all-editable static-metadata workspaces, and tests only include a single main workspace, ignoring path dependencies in another workspace. This can be considered the MVP for workspace support: You can create a workspace, you can install from it, but some options and conveniences are still missing. I'll file follow-up tickets (support in lockfiles, support path deps in other workspace, #3625)

There is also support in `uv run`, but we need https://github.com/astral-sh/uv/issues/3700 first to properly support using different current projects in the bluejay interface, currently the resolution and therefore the lockfile depends on the current project. I'd do this change first (it's big enough already), then #3700, and then add workspace support properly to bluejay.

Fixes #3404

---

_Label `preview` added by @konstin on 2024-05-22 12:37_

---

_Comment by @codspeed-hq[bot] on 2024-05-22 12:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/workspaces)

### Merging #3705 will **not alter performance**

<sub>Comparing <code>konsti/workspaces</code> (273b3f6) with <code>main</code> (89cfece)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Marked ready for review by @konstin on 2024-05-22 19:12_

---

_@konstin reviewed on 2024-05-23 11:52_

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:174 on 2024-05-23 11:52_

These are mostly async for consistency

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:80 on 2024-05-23 23:20_

Nit: Lets avoid abbreviation (`Dep` to `Dependency`)

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:413 on 2024-05-23 23:20_

Nit: "reclusive", should be it be "recursive"?

---

_Review comment by @charliermarsh on `crates/uv/tests/workspace.rs`:173 on 2024-05-23 23:22_

Can we just come up with an example that uses fewer, smaller packages? E.g., do we really need boltons, which is almost 200 KB?

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:154 on 2024-05-23 23:22_

Nit: "exiting" should be "existing"

---

_@charliermarsh reviewed on 2024-05-23 23:23_

The discovery logic looks good to me. Just a few comments.

---

_@zanieb reviewed on 2024-05-24 21:29_

---

_Review comment by @zanieb on `crates/uv-requirements/src/workspace.rs`:32 on 2024-05-24 21:29_

I'd prefer `DynamicSection` or `DynamicNotAllowed`

---

_Comment by @charliermarsh on 2024-05-25 13:26_

> The workspace tests run fully offline using a directory index, this adds 648KB in the links directory. We could reduce this by switching back to setuptools or using even smaller deps (or just gut the existing ones). Those tests being offline makes them faster, easier to debug (no noise from the network requests) and avoids accidentally pulling a pypi package of the same name.

@konstin - sorry for going back-and-forth on this, but can we remove all the added links and just install from PyPI? I don't think we should change our test policy in a PR like this -- we pull directly for other tests, so it seems correct here. And checking-in binaries has its own downsides and risks.


---

_@charliermarsh reviewed on 2024-05-27 04:17_

---

_Review comment by @charliermarsh on `scripts/links/tqdm-4.66.4-py3-none-any.whl`:1 on 2024-05-27 04:17_

Is this still necessary?

---

_@charliermarsh reviewed on 2024-05-27 04:18_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:580 on 2024-05-27 04:18_

When does this occur? I'm looking at this from the perspective of: what regressions could we ship that would accidentally impact users?

---

_@konstin reviewed on 2024-05-27 10:24_

---

_Review comment by @konstin on `scripts/links/tqdm-4.66.4-py3-none-any.whl`:1 on 2024-05-27 10:24_

Removed it

---

_@konstin reviewed on 2024-05-27 10:24_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:580 on 2024-05-27 10:24_

This branch is only reachable when using `uv.tool.sources` (otherwise source is `None`), so it's preview gated

---

_@charliermarsh approved on 2024-05-27 15:05_

---

_@charliermarsh reviewed on 2024-05-27 15:07_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:1 on 2024-05-27 15:07_

Should all of this logic instead be in `source_tree.rs`, like in the source tree resolver, instead of defined here?

---

_@charliermarsh reviewed on 2024-05-27 15:07_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:710 on 2024-05-27 15:07_

Why unwrap when the method itself already returns `Result`? Can we be consistent here?

---

_@charliermarsh reviewed on 2024-05-27 15:07_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/pyproject.rs`:580 on 2024-05-27 15:07_

Can you please expand the TODO?

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:1 on 2024-05-27 15:57_

What parts do you mean? If it's a larger refactoring, i'd do it in another PR

---

_@konstin reviewed on 2024-05-27 15:57_

---

_@charliermarsh reviewed on 2024-05-27 16:41_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:1 on 2024-05-27 16:41_

I'm wondering if we shouldn't be doing _any_ dependency resolution in `specification.rs` and, instead, move all requirement collection into `source_tree.rs`.

---

_@charliermarsh reviewed on 2024-05-27 16:42_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/specification.rs`:1 on 2024-05-27 16:42_

(Not for this PR, I suppose.)

---

_Merged by @konstin on 2024-05-28 07:41_

---

_Closed by @konstin on 2024-05-28 07:41_

---

_Branch deleted on 2024-05-28 07:41_

---
