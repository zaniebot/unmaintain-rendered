```yaml
number: 5163
title: propagate markers to sibling dependencies of forks
type: pull_request
state: merged
author: BurntSushi
labels:
  - resolver
assignees: []
merged: true
base: main
head: ag/fix-torch-marker-bug
created_at: 2024-07-17T18:22:23Z
updated_at: 2024-07-26T14:28:22Z
url: https://github.com/astral-sh/uv/pull/5163
synced_at: 2026-01-12T16:06:40Z
```

# propagate markers to sibling dependencies of forks

---

_@BurntSushi_

When a fork occurs, we divide not just the dependencies that
provoked a fork into distinct groups, but we also add the
corresponding sibling dependencies to each fork. Previously,
while we track markers on the fork itself, the individual
dependencies that had markers only corresponded to markers
written from the dependency specification.

This meant that the sibling dependencies that got added to
each fork would not themselves have markers attached to them.
This in turn meant they would not have markers associated with
them in the lock file.

In many cases, this is actually okay, because the resolver will
pick a version that is "universal" across all forks in most
cases. But in some cases, this just simply isn't possible as
the marker expressions in the fork can and do influence resolution.
In which case, it is possible for the same package with different
versions to show up in the lock file unconditionally. Which is a
big no-no.

So in this commit, after we determine the forks, we intersect the
markers on each fork with each of its dependencies.

This does seem to balloon the marker expressions in some cases.
I plucked one low hanging fruit to avoid doing `x and x` in
trivial cases. (And this eliminated a portion of the snapshot
diffs.) But some pretty gnarly diffs remain. So we will likely
want to invest more in marker simplification.

Fixes #5086, Fixes #5162

Partially addresses #4732

---

_Review requested from @konstin by @BurntSushi on 2024-07-17 18:22_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-17 18:22_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/package.rs`:48 on 2024-07-17 18:23_

I am unsure if this is actually true.

---

_@BurntSushi reviewed on 2024-07-17 18:23_

---

_@BurntSushi reviewed on 2024-07-17 18:23_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/package.rs`:163 on 2024-07-17 18:23_

I am unsure of what to do in this case. I haven't given it too much thought yet.

---

_@BurntSushi reviewed on 2024-07-17 18:24_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:2906 on 2024-07-17 18:24_

Brutal

---

_@BurntSushi reviewed on 2024-07-17 18:24_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6983 on 2024-07-17 18:24_

I am unsure if this new snapshot is correct. The marker expressions look awfully suspicious here.

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:1922 on 2024-07-17 18:24_

This test (and the one below) is what inspired me to find #5161 and #5162.

---

_@BurntSushi reviewed on 2024-07-17 18:24_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:1922 on 2024-07-17 18:25_

Unfortunately, the status quo is _correct_, because this update leaves a "hole" (e.g., on Windows) where previously `a` would be installed but now it isn't.

---

_@BurntSushi reviewed on 2024-07-17 18:25_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/dependencies.rs`:33 on 2024-07-18 07:38_

Can we make those `Fn(Self, T) -> Self` instead? iirc we don't pass `&mut PubGrubDependency` around, so only the owner can mutate them anyway.

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/package.rs`:142 on 2024-07-18 07:41_

We should be doing those changes directly after instance creation and before passing it pubgrub (which clones the package), so this should hold true

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/package.rs`:163 on 2024-07-18 07:44_

I could happen if you fork at the root and that determines whether to use a workspace package which has dev deps

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:2906 on 2024-07-18 07:46_

Is it time for some resolution

---

_Marked ready for review by @konstin on 2024-07-18 08:18_

---

_@konstin approved on 2024-07-18 08:18_

---

_Converted to draft by @konstin on 2024-07-18 08:18_

---

_@BurntSushi reviewed on 2024-07-24 17:50_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:33 on 2024-07-24 17:50_

Yes, but `MarkerTree::and` takes `&mut Self`.

I think either way can work where we avoid cloning `MarkerTree`. This way, it means using `Arc::make_mut`. If we took ownership, it would be `Arc::unwrap_or_clone`. But, in the `make_mut` case where there is only one reference, we don't need to re-create the `Arc`.

How much this matters, I dunno. But I opted for the option that I thought was less costly. (And there are assuredly designs that are even less costly than what I have, but I think it would be a bigger refactor.)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/package.rs`:142 on 2024-07-24 17:51_

Great, thank you. I added that to this comment.

---

_@BurntSushi reviewed on 2024-07-24 17:51_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:7962 on 2024-07-25 14:13_

This is a regression test for #5086.

---

_@BurntSushi reviewed on 2024-07-25 14:13_

---

_Marked ready for review by @BurntSushi on 2024-07-25 14:14_

---

_Label `resolver` added by @BurntSushi on 2024-07-25 14:37_

---

_Review requested from @konstin by @BurntSushi on 2024-07-25 14:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-25 15:47_

Shouldn't this be a single fork that we build by using the original deps and removing all that are disjoint with this fork's markers?

---

_@konstin reviewed on 2024-07-25 15:48_

---

_@BurntSushi reviewed on 2024-07-25 16:00_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-25 16:00_

I don't think so. That was how I had originally done it (except without adding original deps). Crucially, this new "remaining universe" partitioning is added in the same way as other partitionings are. That is, it needs to be considered in the context of any extant forks. Basically, when a fork is created, it starts by copying all extant forks at the time the partitioning was determined, adding dependencies corresponding to the fork along with the markers.

Let me see if I can come up with an example using packse.

---

_@BurntSushi reviewed on 2024-07-25 16:40_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-25 16:40_

OK, I _think_ I came up with an example here: https://github.com/astral-sh/packse/pull/204

For that scenario, I believe your suggestion would lead to the "remaining universe" fork created by `b` in the dependencies of `a` having a marker expression of just `os_name != 'linux' and os_name != 'darwin'`. But it actually should be combined with the marker expressions of every parent fork.

---

_@konstin reviewed on 2024-07-26 10:05_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-26 10:05_

The scenario doesn't seem to use that code path, it looks like with it we're only doing one iteration with the parent universal fork:

```rust
if let Some(markers) = fork_groups.remaining_universe() {
    trace!("Adding split to cover possibly incomplete markers: {markers}");
    println!(
        "forks: {} {}",
        forks.len(),
        forks
            .iter()
            .map(|fork| format!("({})", fork.markers))
            .join(" || ")
    );
    let mut new_forks_for_remaining_universe = forks.clone();
    for fork in &mut new_forks_for_remaining_universe {
        fork.markers.and(markers.clone());
        fork.dependencies.retain(|dep| {
            let Some(dep_marker) = dep.package.marker() else {
                return true;
            };
            // After we constrain the markers on an existing
            // fork, we should ensure that any existing
            // dependencies that are no longer possible in this
            // fork are removed. This mirrors the check we do in
            // `add_nonfork_package`.
            !crate::marker::is_disjoint(&fork.markers, dep_marker)
        });
    }
    println!(
        "new_forks_for_remaining_universe: {} {}",
        new_forks_for_remaining_universe.len(),
        new_forks_for_remaining_universe
            .iter()
            .map(|fork| format!("({})", fork.markers))
            .join(" || ")
    );
    new_forks.extend(new_forks_for_remaining_universe);
}
```

```console
$ rm -f uv.lock && cargo run lock -v 2> a.txt
forks: 1 ()
new_forks_for_remaining_universe: 1 (sys_platform != 'illumos' and sys_platform != 'windows')
forks: 1 ()
new_forks_for_remaining_universe: 1 (os_name != 'darwin' and os_name != 'linux')
```

---

_@konstin reviewed on 2024-07-26 14:09_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-26 14:09_

We will merge this as-is and follow-up on it later, overlapping markers are more important to fix.

---

_@BurntSushi reviewed on 2024-07-26 14:15_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2738 on 2024-07-26 14:15_

You're 100% correct. It doesn't! I even had my own debug prints and was not careful in looking at what they were telling me. I spent some time trying to up with a better example, but failed. I filed https://github.com/astral-sh/uv/issues/5482 to track investigation into this possible issue.

---

_Merged by @BurntSushi on 2024-07-26 14:28_

---

_Closed by @BurntSushi on 2024-07-26 14:28_

---

_Branch deleted on 2024-07-26 14:28_

---
