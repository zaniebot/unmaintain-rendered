```yaml
number: 2578
title: Add support for unnamed Git and HTTP requirements
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bare-vi
created_at: 2024-03-20T22:57:09Z
updated_at: 2024-03-21T13:44:55Z
url: https://github.com/astral-sh/uv/pull/2578
synced_at: 2026-01-10T14:49:08Z
```

# Add support for unnamed Git and HTTP requirements

---

_Pull request opened by @charliermarsh on 2024-03-20 22:57_

## Summary

Enables, e.g., `uv pip install git+https://github.com/pallets/flask.git`.

Part of: https://github.com/astral-sh/uv/issues/313.


---

_Marked ready for review by @charliermarsh on 2024-03-20 23:41_

---

_Review requested from @konstin by @charliermarsh on 2024-03-20 23:43_

---

_@zanieb reviewed on 2024-03-21 02:09_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:09_

Should we just be extracting the files of interest here? Or is that not worthwhile because we extract the whole thing later for installation and we're caching extracting for the duration of this process?

---

_@charliermarsh reviewed on 2024-03-21 02:11_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:11_

Yeah, this is very wasteful (we don't cache this, we have to re-extract later, but this is only for HTTPS requirements that don't reference a specific built file, I don't know that this really even happens in practice). I could change it to only extract the files of interest. The downside is that if we _do_ want to use PEP 517 hooks (which we're "supposed to"), then we need to do the full extraction anyway.

---

_@zanieb reviewed on 2024-03-21 02:12_

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:1247 on 2024-03-21 02:12_

Does it feel worth clarifying that extraction occurs in the name here? It was not obvious until I pulled up an IDE to look at the type signature.

---

_@charliermarsh reviewed on 2024-03-21 02:14_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:14_

(Aside) It's just really hard to cache here in a way that can be reused later because we don't have a `Dist`, and all the caching layers require `Dist`. Like, ideally, we could just use the `DistributionDatabase` for this, but that requires `Dist` everywhere.

---

_@zanieb reviewed on 2024-03-21 02:22_

---

_Review comment by @zanieb on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:22_

I'd be okay with only extracting a subset of files in a future optimization. I think regardless we'd only want to use a PEP 517 hook _after_ our fast heuristics are exhausted, right?

What are we missing from a `Dist`? Just the name? Can we put it in the cache after we've acquired the name?

---

_@charliermarsh reviewed on 2024-03-21 02:29_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:29_

In theory yes but executing on that is pretty tricky given the way the code is structured. I plan to try it...

The other option is that we could _remove_ the package name from the cache, and just use the URL as a key. This would make things much easier. (And it would mean we could _read_ from the cache here, not just write.) The main downside (AFAICT) is that it would (1) break the cache so we'd need to bump version, and (2) break `uv cache clean flask` so we'd need to figure out how to support that (it's possible).


---

_@charliermarsh reviewed on 2024-03-21 02:37_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:37_

I think it's doable but it's _much_ easier if we remove the package name from the cache entries.

---

_@charliermarsh reviewed on 2024-03-21 02:45_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:589 on 2024-03-21 02:45_

Ok, first I'm gonna make it so we don't unnecessarily extract... Then I will look into caching these...

---

_Comment by @charliermarsh on 2024-03-21 04:32_

I'm comfortable merging this as-is, but the next step I'm working on is to (1) use PEP 517 builds for this, and (2) ensure that the built artifacts are cached.

---

_Review requested from @zanieb by @charliermarsh on 2024-03-21 04:36_

---

_Review comment by @konstin on `crates/uv/src/requirements.rs`:536 on 2024-03-21 10:04_

Do we need to sort those? iirc the order here determines the prioritization in pubgrub.

---

_@konstin approved on 2024-03-21 10:06_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:536 on 2024-03-21 13:28_

Good call...

---

_@charliermarsh reviewed on 2024-03-21 13:28_

---

_Merged by @charliermarsh on 2024-03-21 13:44_

---

_Closed by @charliermarsh on 2024-03-21 13:44_

---

_Branch deleted on 2024-03-21 13:44_

---
