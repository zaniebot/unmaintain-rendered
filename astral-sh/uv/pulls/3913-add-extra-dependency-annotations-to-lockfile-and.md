```yaml
number: 3913
title: Add extra dependency annotations to lockfile and sync commands
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extras
created_at: 2024-05-29T17:15:02Z
updated_at: 2024-05-29T19:26:27Z
url: https://github.com/astral-sh/uv/pull/3913
synced_at: 2026-01-12T16:05:55Z
```

# Add extra dependency annotations to lockfile and sync commands

---

_@charliermarsh_

## Summary

This PR adds extras to the lockfile, and enables users to selectively sync extras in `uv sync` and `uv run`. The end result here was fairly simple, though it required a few refactors to get here. The basic idea is that `DistributionId` now includes `extra: Option<ExtraName>`, so we effectively treat extras as separate packages. Generating the lockfile, and generating the resolution from the lockfile, fall out of this naturally with no special-casing or additional changes.

The main downside here is that it bloats the lockfile significantly. Specifically:

- We include _all_ distribution URLs and hashes for _every_ extra variant.
- We include all dependencies for the extra variant, even though that are dependencies of the base package.

We could normalize this representation by changing each distribution have an `optional-dependencies` hash map that keys on extras, but we actually don't have the information we need to create that right now (specifically, we can't differentiate between dependencies that _require_ the extra and dependencies on the base package).

Closes #3700.


---

_Marked ready for review by @charliermarsh on 2024-05-29 17:15_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-29 17:15_

---

_Review requested from @konstin by @charliermarsh on 2024-05-29 17:15_

---

_@charliermarsh reviewed on 2024-05-29 18:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:673 on 2024-05-29 18:11_

This demonstrates the change in format. We now have a separate distribution for `project[test]`, which includes the dependencies for both the base package and the `[test]`.

I think we could change the `project` distribution to instead include `[[distribution.optional-dependencies.test]]`? But it requires more work.

---

_Comment by @codspeed-hq[bot] on 2024-05-29 18:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/extras)

### Merging #3913 will **not alter performance**

<sub>Comparing <code>charlie/extras</code> (cd6a179) with <code>main</code> (1bd5d8b)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolution/graph.rs`:263 on 2024-05-29 19:03_

How come this check isn't needed any more?

---

_@BurntSushi approved on 2024-05-29 19:04_

I think this LGTM for now, but I suspect we'll definitely want to pursue some kind of normalization here.

I'm also not quite fully sold on having a distinct package for each `extra`, but I don't have a good alternative to propose at the moment.

---

_Comment by @charliermarsh on 2024-05-29 19:07_

I can look into normalizing it as a next step. It may require changing some things in the resolver itself to make it _possible_ to track dependencies that were only pulled in due to an extra.

---

_@charliermarsh reviewed on 2024-05-29 19:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:263 on 2024-05-29 19:08_

I think this was made redundant in a prior change, when we introduced `PubGrubPackageInner::Extra`.

---

_@charliermarsh reviewed on 2024-05-29 19:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/graph.rs`:263 on 2024-05-29 19:09_

Because this is no longer true. The requiring package won't be `PubGrubPackageInner::Package`, but `PubGrubPackageInner::Extra`.

---

_Merged by @charliermarsh on 2024-05-29 19:25_

---

_Closed by @charliermarsh on 2024-05-29 19:25_

---

_Branch deleted on 2024-05-29 19:26_

---
