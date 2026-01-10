```yaml
number: 2471
title: "Make > operator exclude post and local releases"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/max-version
created_at: 2024-03-15T02:20:19Z
updated_at: 2024-03-15T14:02:07Z
url: https://github.com/astral-sh/uv/pull/2471
synced_at: 2026-01-10T14:49:08Z
```

# Make > operator exclude post and local releases

---

_Pull request opened by @charliermarsh on 2024-03-15 02:20_

## Summary

This PR attempts to use a similar trick to that we added in https://github.com/astral-sh/uv/pull/1878, but for post-releases.

In https://github.com/astral-sh/uv/pull/1878, we added a fake "minimum" version to enable us to treat `< 1.0.0` as _excluding_ pre-releases of 1.0.0.

Today, on `main`, we accept post-releases and local versions in `> 1.0.0`. But per PEP 440, that should _exclude_ post-releases and local versions, unless the specifier is itself a pre-release, in which case, pre-releases are allowed (e.g., `> 1.0.0.post0` should allow `> 1.0.0.post1`).

To support this, we add a fake "maximum" version that's greater than all the post and local releases for a given version. This leverages our last remaining free bit in the compact representation.


---

_Label `bug` added by @charliermarsh on 2024-03-15 02:20_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:2419 on 2024-03-15 02:23_

Mildly concerned with how all of this intersects with post-releases, but I did add tests.

---

_@charliermarsh reviewed on 2024-03-15 02:23_

---

_@charliermarsh reviewed on 2024-03-15 02:25_

---

_Review comment by @charliermarsh on `scripts/scenarios/generate.py`:207 on 2024-03-15 02:25_

This is the payoff, we can remove a bunch of incorrectly-succeeding scenarios.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-15 02:26_

---

_Comment by @charliermarsh on 2024-03-15 02:27_

This is related to https://github.com/astral-sh/uv/pull/2430, but not really relevant for the core use-case of local versions (PyTorch) -- it's just a correctness thing.

I _do_ think we would've received a bug report that `> 1.0.0` should not match `> 1.0.0.post0` at some point though. I'm actually surprised that hasn't come up yet.

---

_@charliermarsh reviewed on 2024-03-15 02:39_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:2419 on 2024-03-15 02:39_

I ended up removing `dev` support, we never create a version with both `max` and `dev` so not sure it's worth accommodating it here.

---

_@charliermarsh reviewed on 2024-03-15 02:40_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:2421 on 2024-03-15 02:40_

We could also consider incorporating `max` into the `match` below... This felt easier to get right.

---

_@zanieb reviewed on 2024-03-15 03:52_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:1517 on 2024-03-15 03:52_

Not for here, but I wonder if we should add a "hint" explaining why `package-b==2.0.0+foo` does not satisfy `package-b>2.0.0`

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:2125 on 2024-03-15 03:54_

Similarly, I wonder if we should add a hint that `1.2.3.post1` is available. I think it's probably unnecessary here since `post` releases feel equivalent to the version they follow and are generally transparent to users?

---

_@zanieb reviewed on 2024-03-15 03:54_

---

_@zanieb approved on 2024-03-15 03:58_

Makes sense to me!

It might be nice to have scenarios for:

- The user requests a post version but there are no post versions available
- The user requests a greater than post version the excludes all available post versions

To ensure we display these correctly in our unsat messages

---

_@zanieb reviewed on 2024-03-15 03:59_

---

_Review comment by @zanieb on `crates/pep440-rs/src/version.rs`:2419 on 2024-03-15 03:59_

Is this caveat documented?

---

_@BurntSushi reviewed on 2024-03-15 12:57_

This LGTM, but I think you might want to bump the cache version because of the representational change to `Version`. Actually, I kind of wonder if you maybe don't need to do it for this change, since I don't believe any existing versions have their meaning changed or become invalidated. Unlike the `min` change, this change doesn't impact the existing suffix tags. Unless we're 100% sure, we should bump it, because if we're wrong it can cause subtle and implicit bugs.

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version.rs`:2419 on 2024-03-15 13:49_

I added debug asserts to the constructors for `min` and `max`.

---

_@charliermarsh reviewed on 2024-03-15 13:49_

---

_Comment by @charliermarsh on 2024-03-15 13:49_

Bumped cache!

---

_@BurntSushi approved on 2024-03-15 13:52_

---

_Merged by @charliermarsh on 2024-03-15 14:02_

---

_Closed by @charliermarsh on 2024-03-15 14:02_

---

_Branch deleted on 2024-03-15 14:02_

---
