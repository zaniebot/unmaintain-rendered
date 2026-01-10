```yaml
number: 3958
title: Normalize extras in lockfile 
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-06-01T18:13:57Z
updated_at: 2024-06-03T19:00:36Z
url: https://github.com/astral-sh/uv/pull/3958
synced_at: 2026-01-10T13:54:02Z
```

# Normalize extras in lockfile 

---

_Pull request opened by @charliermarsh on 2024-06-01 18:13_

## Summary

Previously, when we locked something like `flask[dotenv]`, we created two separate distributions in the lockfile: one for `flask`, which included the base dependencies, and one for `flask[dotenv]`, which included the base dependencies _and_ the `dotenv` dependencies. This was easy to implement, but it meant that we were duplicating all of the distribution files for every extra, and duplicating all of the base dependencies for every extra.

This PR normalizes the data such that we now have one entry per distribution (i.e., `ExtraName` was removed from `DistributionId`), with an optional dependencies table with an entry per extra, like:

```toml
[[distribution]]
name = "project"
version = "0.1.0"
source = "editable+file://[TEMP_DIR]/"
sdist = { url = "file://[TEMP_DIR]/" }

[[distribution.dependencies]]
name = "anyio"
version = "3.7.0"
source = "registry+https://pypi.org/simple"

[distribution.optional-dependencies]

[[distribution.optional-dependencies.test]]
name = "iniconfig"
version = "2.0.0"
source = "registry+https://pypi.org/simple"
```

This requires a bit more work upfront, because we now need to merge multiple packages from the `PetGraph` representation when creating the lockfile.

Closes https://github.com/astral-sh/uv/issues/3916.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-01 18:16_

---

_Marked ready for review by @charliermarsh on 2024-06-01 18:16_

---

_@charliermarsh reviewed on 2024-06-01 18:17_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:546 on 2024-06-01 18:17_

Not thrilled that I'm making so many things `pub(crate)` here. Should I change `ResolutionGraph::to_lock` into a `impl TryFrom<ResolutionGraph` for `Lock`?

---

_Comment by @charliermarsh on 2024-06-01 18:17_

@BurntSushi - Ignoring the code, curious if you prefer this representation?

---

_Comment by @codspeed-hq[bot] on 2024-06-01 18:26_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/ex)

### Merging #3958 will **not alter performance**

<sub>Comparing <code>charlie/ex</code> (02f3ead) with <code>main</code> (362b00c)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Label `preview` added by @charliermarsh on 2024-06-01 18:28_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-01 18:28_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolution/graph.rs`:461 on 2024-06-03 16:41_

Why could the previous code do an unconditional `push` here?

---

_@ibraheemdev approved on 2024-06-03 16:46_

The new representation generally makes sense to me.

Would it make more sense to put optional dependencies under `distributions.extras."name".dependencies`? Could we ever want to put other information under `distributions.extras."name"`?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:546 on 2024-06-03 17:16_

I kind of bias toward concrete and bespoke (but conventional) conversion routines like `ResolutionGraph::to_lock` unless there's a specific need for generic fallible conversions.

I think this is more of a stylistic choice, but I'd say it's just a specific manifestation of "don't go generic unless there's a reason to." And with specific conversion routines, it's straight-forward to add more parameters if they ever become necessary.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:546 on 2024-06-03 17:19_

Ooooooooo, I completely misunderstood your question. You were suggesting the `TryFrom` impl so that the conversion would be defined in this module, and that would in turn prevent exposing stuff.

Yeah I think I'd do that. Although, following from my previous comment, I'd probably just define `Lock::from_resolution_graph` or something. And that might in turn require exposing more stuff from the graph, but maybe that's okay.

(I don't think we've really settled on a great balance here. I wonder, for example, whether it really makes sense to have a `ResolutionGraph` at all. But this gets to the "installation might want different types" idea that's been floating around. It's a bigger refactor for sure.)

---

_@BurntSushi approved on 2024-06-03 17:20_

I think I'd echo @ibraheemdev's question. Otherwise, this generally LGTM. If it's possible, I think I would prefer a way where we're not slapping `pub(crate)` on everything, but I don't feel too strongly at this point while we're still trying to figure out what the data types should be.

---

_Comment by @charliermarsh on 2024-06-03 18:38_

I think I will leave the representation as-is for now because it closely mirrors the `pyproject.toml` schema, where you have an `optional-dependencies` map that's keyed on extra name. That was intentional, because I'm hoping to make it possible to reify the distribution metadata from the lockfile in the future. It's a very good question though.

---

_@charliermarsh reviewed on 2024-06-03 18:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:546 on 2024-06-03 18:40_

Yeah I'm also not totally convinced that `ResolutionGraph` will be necessary in the long run.

---

_Merged by @charliermarsh on 2024-06-03 19:00_

---

_Closed by @charliermarsh on 2024-06-03 19:00_

---

_Branch deleted on 2024-06-03 19:00_

---
