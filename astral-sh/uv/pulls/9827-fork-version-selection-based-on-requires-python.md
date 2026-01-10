```yaml
number: 9827
title: "Fork version selection based on `requires-python` requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/max-version-split
created_at: 2024-12-11T23:15:23Z
updated_at: 2024-12-13T21:06:44Z
url: https://github.com/astral-sh/uv/pull/9827
synced_at: 2026-01-10T12:00:01Z
```

# Fork version selection based on `requires-python` requirements

---

_Pull request opened by @charliermarsh on 2024-12-11 23:15_

## Summary

This PR addresses a significant limitation in the resolver whereby we avoid choosing the latest versions of packages when the user supports a wider range.

For example, with NumPy, the latest versions only support Python 3.10 and later. If you lock a project with `requires-python = ">=3.8"`, we pick the last NumPy version that supported Python 3.8, and use that for _all_ Python versions. So you get `1.24.4` for all versions, rather than `2.2.0`. And we'll never upgrade you unless you bump your `requires-python`. (Even worse, those versions don't have wheels for Python 3.12, etc., so you end up building from source.)

(As-is, this is intentional. We optimize for minimizing the number of selected versions, and the current logic does that well!)

Instead, we know recognize when a version has an elevated `requires-python` specifier and fork. This is a new fork point, since we need to fork once we have the package metadata, as opposed to when we see the dependencies.

In this iteration, I've made this behavior the default. I'm sort of undecided on whether I want to push on that... Previously, I'd suggested making it opt-in via a setting (https://github.com/astral-sh/uv/pull/8686).

Closes https://github.com/astral-sh/uv/issues/8492.


---

_Review requested from @konstin by @charliermarsh on 2024-12-11 23:15_

---

_Review request for @konstin removed by @charliermarsh on 2024-12-11 23:15_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-11 23:15_

---

_Review request for @BurntSushi removed by @charliermarsh on 2024-12-11 23:15_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-11 23:15_

---

_Review requested from @konstin by @charliermarsh on 2024-12-11 23:15_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-11 23:15_

---

_Label `enhancement` added by @charliermarsh on 2024-12-11 23:15_

---

_Comment by @charliermarsh on 2024-12-11 23:17_

I suggest taking a look at the changes in the `lock.rs` tests.

---

_Renamed from "Fork version selection based on requires-python requirements" to "Fork version selection based on `requires-python` requirements" by @charliermarsh on 2024-12-11 23:17_

---

_Comment by @zanieb on 2024-12-12 03:50_

Is this breaking as a default behavior? Or will it not invalidate existing lockfiles?

---

_Comment by @charliermarsh on 2024-12-12 03:55_

No, it wouldn't invalidate existing lockfiles.

---

_Comment by @charliermarsh on 2024-12-12 04:05_

From the test changes, I think this behavior is actually a bit more intuitive.

---

_Review comment by @konstin on `crates/uv-distribution-types/src/prioritized_distribution.rs`:67 on 2024-12-12 10:25_

Can't we read the requires python from disk for it, or is that irrelevant here?

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:327 on 2024-12-12 10:36_

This should get a comment that we're allowed to skip unit propagation because we just forked after package prioritization but before (at least in the fork it's before) version selection.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:358 on 2024-12-12 10:37_

```suggestion
                        // Choose a package.
```

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:3452 on 2024-12-12 10:46_

imo we should only split at `major.minor` and ignore patch levels (see also https://discuss.python.org/t/requires-python-and-pre-release-python-versions/62959/24).

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:4941 on 2024-12-12 10:58_

Note to self: the cp313 wheels aren't missing, this is just our exclude newer

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1129 on 2024-12-12 11:05_

maybe something that we handle implicitly, but wouldn't we always return at least one fork (because union(left, right) is the entire requires-python of the fork we're in, so partitioning it at least one partition must overlap), so when we get one fork we should also continue implicitly?

---

_@konstin approved on 2024-12-12 11:10_

This is elegant!

I do like this as a default. I'm unsure if it's the right default when there is a universal wheel, but it's definitely better for packages with platform specific wheels such as numpy, where it solves a class of problems that is otherwise hard to understand and fix for the user with convenient defaults.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:113 on 2024-12-12 14:08_

A few unit tests for this routine might be worthwhile. :-)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:460 on 2024-12-12 14:11_

How come this isn't just a free function?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:467 on 2024-12-12 14:12_

Is it worth a `TRACE`-level log message here?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1129 on 2024-12-12 14:16_

I had a similar thought here. But I find it somewhat difficult to reason about invariants that are always true. For example, if the fork has a marker that always evaluates to `false`, then `VersionForker::fork` could return an empty set of forks. But... I think it's impossible at this point to have such a marker?

Other than that, I think I agree with you. We could add an `assert!` here.

---

_@BurntSushi approved on 2024-12-12 14:18_

I'm a little uneasy about making this the default (I see that the markers get a fair bit more complicated), but I do agree that this does seem to work well in the Python ecosystem. I trust your judgment. :)

---

_@konstin reviewed on 2024-12-12 14:42_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:467 on 2024-12-12 14:42_

Would also be helpful to have a debug log when we hit the `return Ok(Some(ResolverVersion::Forked(forks)));` branch so we log that we fork due to requires python

---

_Comment by @konstin on 2024-12-12 15:07_

We should add a sentence or two about this new behavior in `resolver-internals.md`.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1129 on 2024-12-12 21:30_

Both of the forks need to be non-empty when splitting the Python requirement. We then filter out forks that are disjoint with the markers, because it's subtly different... We could have a project with `requires-python = ">=3.9"`, and then be in a fork with `python_version < '3.8' or python_version > '3.11`.

If we then split at Python 3.10, we'd split the requires-python into `>=3.9, <3.10` and `>=3.10`.

If we intersect `>=3.9, <3.10` with `python_version < '3.8' or python_version > '3.11`, that's empty, so we need to drop that piece.


---

_@charliermarsh reviewed on 2024-12-12 21:30_

---

_@charliermarsh reviewed on 2024-12-13 01:22_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:67 on 2024-12-13 01:22_

I believe it's irrelevant here.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:113 on 2024-12-13 02:50_

Good call!

---

_@charliermarsh reviewed on 2024-12-13 02:50_

---

_Comment by @charliermarsh on 2024-12-13 04:14_

Current thinking is to make this the default, but add a setting such that users can opt-out if necessary.

---

_Merged by @charliermarsh on 2024-12-13 20:33_

---

_Closed by @charliermarsh on 2024-12-13 20:33_

---

_Branch deleted on 2024-12-13 20:33_

---

_Comment by @henryiii on 2024-12-13 21:05_

Minor comment on the PR summary: NumPy now only supports 3.11+, no longer 3.10+ (following SPEC 0). ;)

---
