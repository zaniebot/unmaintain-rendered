```yaml
number: 5597
title: "uv-resolver: fix marker propagation"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-marker-propagation
created_at: 2024-07-30T12:42:26Z
updated_at: 2024-08-23T14:47:15Z
url: https://github.com/astral-sh/uv/pull/5597
synced_at: 2026-01-10T13:09:50Z
```

# uv-resolver: fix marker propagation

---

_Pull request opened by @BurntSushi on 2024-07-30 12:42_

This PR represents a different approach to marker propagation in an
attempt to unblock #5583. In particular, instead of propagating markers
when forks are created, we wait until resolution is complete to
propagate all markers to all dependencies in each fork. This ends up
being both more robust (we should never miss anything) and simpler to
implement because it doesn't require mutating a `PubGrubPackage` (which
was pretty annoying). I think the main downside here is that this can
sometimes add markers where they aren't needed.

This actually winds up making quite a few snapshot changes. I went
through each of them. Some of them look like legitimate bug fixes. Some
of them look like superfluous additions. And some of them look like they
would be removed if we had perfect marker normalization. But I don't
think any of the changes are _wrong_.


---

_@charliermarsh approved on 2024-07-30 12:44_

---

_Review comment by @BurntSushi on `crates/uv/tests/branching_urls.rs`:236 on 2024-07-30 12:44_

I think these are superfluous because the dependency on `anyio` from `a` above is already gated on marker expressions.

(I think this change overall brings us closer to "write out the full marker expressions for each distribution" that could potentially obviate the need for a DAG traversal.)

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:2926 on 2024-07-30 12:45_

I believe these are all correct simplifications from what was here previously.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:585 on 2024-07-30 12:47_

I think this one and the one above are superfluous (again, assuming a DAG traversal).

---

_@konstin approved on 2024-07-30 12:48_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:2752 on 2024-07-30 12:51_

This one looks like a legitimate bug fix, since previous we had unconditional dependencies on two different versions of `package-foo` here. I filed https://github.com/astral-sh/uv/issues/5598 in reaction to this.

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6615 on 2024-07-30 12:52_

I believe the extra term added here is strictly redundant.

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6929 on 2024-07-30 12:52_

This looks like another legitimate bug fix, where previously we could install two different versions of `torch`.

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7733 on 2024-07-30 12:52_

Another bug fix!

---

_@BurntSushi reviewed on 2024-07-30 12:52_

---

_Merged by @BurntSushi on 2024-07-30 13:16_

---

_Closed by @BurntSushi on 2024-07-30 13:16_

---

_Branch deleted on 2024-07-30 13:16_

---
