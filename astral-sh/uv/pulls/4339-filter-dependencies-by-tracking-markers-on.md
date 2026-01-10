```yaml
number: 4339
title: filter dependencies by tracking markers on resolver forks
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/universal-forked-markers
created_at: 2024-06-15T14:08:44Z
updated_at: 2024-06-17T13:30:38Z
url: https://github.com/astral-sh/uv/pull/4339
synced_at: 2026-01-10T13:54:02Z
```

# filter dependencies by tracking markers on resolver forks

---

_Pull request opened by @BurntSushi on 2024-06-15 14:08_

This PR adds the tracking of marker expressions across forked resolver
states. These marker expressions are then used to exlcude dependencies
with non-overlapping marker expressions from consideration in
corresponding forks only.

This PR also fixes a bug I found in marker disjoint checking as part of
testing this change.

Lots of packse tests were added here:
https://github.com/astral-sh/packse/pull/194

Closes #4194


---

_Review requested from @konstin by @BurntSushi on 2024-06-15 14:10_

---

_Review requested from @zanieb by @zanieb on 2024-06-15 14:25_

---

_@zanieb approved on 2024-06-15 14:40_

The implementation looks good to me; I didn't review the lock snapshots.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1553 on 2024-06-17 10:57_

That needs some additional context about the intermediary step, where any marker `A` on a requirement is never disjoint with an empty marker tree (the default here), so we consider the marker tree here truthy

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1892 on 2024-06-17 11:00_

Can multiple dependencies of the same name also exist without forking, e.g. when foo has:

```
Requires-Dist: bar >= 1
Requires-Dist: bar < 2
```

These cases exist e.g. with extras, but sometimes also with non-conflicting environment markers.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2012 on 2024-06-17 11:03_

Nit: missing article(?)

---

_@konstin approved on 2024-06-17 11:34_

---

_@BurntSushi reviewed on 2024-06-17 12:26_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1553 on 2024-06-17 12:26_

Hmmm. Doesn't that immediately follow from the fact that the marker tree is always true for any possible marker environment?

Also, I'm not using "empty marker tree" because there are two conceivable variants of an empty marker tree: `MarkerTree::And(vec![])` and `MarkerTree::Or(vec![])`. The former is always true (which is what matters here) while the latter is always false.

I'll try to add something to the docs here.

---

_@BurntSushi reviewed on 2024-06-17 12:28_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1553 on 2024-06-17 12:28_

OK, I added a paragraph. Hopefully it makes its use a little clearer:

```rust
    /// The marker expression that created this state.
    ///
    /// The root state always corresponds to a marker expression that is always
    /// `true` for every `MarkerEnvironment`.
    ///
    /// In non-universal mode, forking never occurs and so this marker
    /// expression is always `true`.
    ///
    /// Whenever dependencies are fetched, all requirement specifications
    /// are checked for disjointness with the marker expression of the fork
    /// in which those dependencies were fetched. If a requirement has a
    /// completely disjoint marker expression (i.e., it can never be true given
    /// that the marker expression that provoked the fork is true), then that
    /// dependency is completely ignored.
```

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1892 on 2024-06-17 13:25_

Yeah great question. I was careful to make `Dependencies::fork` infallible so that it didn't try to be too eager about reporting errors. The idea is that if there aren't any requirements with the same package with non-overlapping markers, then the result returned by `Dependencies::fork` is unchanged from what it was. It's just a flat list of requirements. That is, no forks. So something like the scenario you outlined will work just as it always has.

One hiccup here though is if you have this:

```
Requires-Dist: bar >= 1 ; sys_platform == 'linux'
Requires-Dist: bar < 2 ; sys_platform == 'darwin'
```

Even though the version constraints are non-conflicting, because the marker expressions are non-overlapping, this will actually provoke a fork. The fork means that the `bar >= 1` requirement is free to choose, say, `bar==2.0.0` if it's available because the fork exists in isolation from the `bar < 2` requirement. However, if no fork were to happen, then both the `bar >= 1` and `bar < 2` requirements would be active simultaneously such that `bar==2.0.0` couldn't be chosen. So in this case, there exists a resolution without forking, but it is distinct from the resolution we get if we fork.

I think we could detect this case by looking at whether the version constraints themselves are conflicting. That is, if there exists an overlap in the version constraints, then we don't fork even if the marker expressions are non-overlapping. I think a decision like that would be predicated on the idea that it could be _possible_ to find one resolution that works across multiple platforms. But it might not be possible. It feels like forking in this case, even if it isn't always necessary, results in more freedom for the resolver to find a resolution. But... maybe it's surprising to users in real cases. I'm not sure.

I added tests for some of these cases here: https://github.com/astral-sh/packse/pull/196

---

_@BurntSushi reviewed on 2024-06-17 13:25_

---

_@BurntSushi reviewed on 2024-06-17 13:29_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1892 on 2024-06-17 13:29_

I created an issue for the non-conflicting requirements with non-overlapping markers scenario: https://github.com/astral-sh/uv/issues/4357

---

_Merged by @BurntSushi on 2024-06-17 13:30_

---

_Closed by @BurntSushi on 2024-06-17 13:30_

---

_Branch deleted on 2024-06-17 13:30_

---
