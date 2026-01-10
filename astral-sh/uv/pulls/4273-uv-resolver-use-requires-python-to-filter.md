```yaml
number: 4273
title: "uv-resolver: use Requires-Python to filter dependencies during universal resolution"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/universal-requires-python
created_at: 2024-06-12T14:19:13Z
updated_at: 2024-06-12T17:30:48Z
url: https://github.com/astral-sh/uv/pull/4273
synced_at: 2026-01-10T13:54:02Z
```

# uv-resolver: use Requires-Python to filter dependencies during universal resolution

---

_Pull request opened by @BurntSushi on 2024-06-12 14:19_

In the time before universal resolving, we would always pass a
`MarkerEnvironment`, and this environment would capture any relevant
`Requires-Python` specifier (including if `-p/--python` was provided on
the CLI).

But in universal resolution, we very specifically do not use a
`MarkerEnvironment` because we want to produce a resolution that is
compatible across potentially multiple environments. This in turn meant
that we lost `Requires-Python` filtering.

This PR adds it back. We do this by converting our `PythonRequirement`
into a `MarkerTree` that encodes the version specifiers in a
`Requires-Python` specifier. We then ask whether that `MarkerTree` is
disjoint with a dependency specification's `MarkerTree`. If it is, then
we know it's impossible for that dependency specification to every be
true, and we can completely ignore it.

Packse tests were added in this PR: https://github.com/astral-sh/packse/pull/187


---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/dependencies.rs`:102 on 2024-06-12 14:22_

```suggestion
        // If the requirement would not be selected with any Python version supported by the root, skip it.
```

(same below)

---

_@konstin approved on 2024-06-12 14:27_

---

_@charliermarsh reviewed on 2024-06-12 15:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-12 15:04_

Should this evaluate to `None` if `requires_python` is empty? What's the thinking behind `MarkerTree::And(vec![])`?

---

_@charliermarsh reviewed on 2024-06-12 15:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:04_

I'm having trouble reasoning about whether we should be using both of these (i.e., both `python_version` and `python_full_version`).

---

_@charliermarsh reviewed on 2024-06-12 15:11_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:11_

Trying to talk it out... Let's say you have `Requires-Python = ">= 3.10.1"`...

Then we'd add `python_version >= "3.10.1"` and `python_full_version >= "3.10.1"`.

Then let's say we see a marker on a dependency `python_version == "3.10"`.

That actually _does_ intersect with `Requires-Python = ">= 3.10.1"`, because by definition in `python_version` we only capture the major and minor (it's computed as: `'.'.join(platform.python_version_tuple()[:2])`). But here we would consider it disjoint from `python_version >= "3.10.1"`.

So, really, we want `python_version >= "3.10"`, not `python_version >= "3.10.1"`?


---

_@charliermarsh reviewed on 2024-06-12 15:12_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:12_

Ok, to simplify things, I might suggest this: use `self.bound` rather than `self.specifiers`. And add `>= self.bound`, and only include the major and minor release for the `MarkerValueVersion::PythonVersion` segment.

---

_@BurntSushi reviewed on 2024-06-12 15:18_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-12 15:18_

I believe it would be correct for this to be `None`. But that's _only_ because we treat `None` and "marker expression is always true" as equivalent. It just makes the contract a little nicer I guess:

```rust
    /// This is derived from `PythonRequirement` once at initialization
    /// time. It's used in universal mode to filter our dependencies with
    /// a `python_version` marker expression that has no overlap with the
    /// `Requires-Python` specifier.
    ///
    /// This is non-None if and only if the resolver is operating in
    /// universal mode. (i.e., when `markers` is `None`.)
    requires_python: Option<MarkerTree>,
```

If we used `None` here then this would need to be rephrased.

I don't have a strong opinion here or anything, but this just made the most sense to me at the time.

---

_@BurntSushi reviewed on 2024-06-12 15:19_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-12 15:19_

(I sometimes wonder if we could get away with not having `Option<MarkerTree>` anywhere.)

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-12 15:21_

I guess I was hoping to avoid ever allowing `And(vec![])` or `Or(vec![])`. Ideally that would be impossible in the future...

---

_@charliermarsh reviewed on 2024-06-12 15:21_

---

_@BurntSushi reviewed on 2024-06-12 15:22_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:22_

The way I see it is if you have `foo ; python_full_version == '3.9'` and `Requires-Python: >=3.10`, then `foo` should be ignored.

That work by transforming `Requires-Python: >=3.10` to `python_version >= '3.10' and python_full_version >= '3.10'`. That marker expression in turn has zero overlap with `python_full_version == '3.9'`, so it is excluded. But if we instead had `bar ; python_full_version == '3.11'`, then there is possible overlap and thus they aren't considered disjoint.

---

_@BurntSushi reviewed on 2024-06-12 15:41_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:200 on 2024-06-12 15:41_

Yeah I had similar hopes for `regex` initially (e.g., banning `""` and `[^\s\S]`), and it just turns out that it's useful to have types like this be able to represent "always match" and "never match." For example, you _might_ be able to ban `And(vec![])`, but 1) you'll have to juggle it with `Option<MarkerTree>` which is annoying and 2) there an infinite number of `MarkerTree` values that have the same truth table as `And(vec![])`. Because of (2), you kinda need to deal with "always true" (and also, "never true") expressions anyway. So you might as well allow the trivial examples of them.

---

_@BurntSushi reviewed on 2024-06-12 15:43_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:43_

(Oops, when I wrote the above comment, I didn't see your two most recent comments. The GitHub UI is maybe slow to update.)

---

_@charliermarsh reviewed on 2024-06-12 15:47_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:47_

Yeah, the basic problem is that `python_version` is definitionally truncated, and I worry that could lead to incorrect reasoning here.

---

_@BurntSushi reviewed on 2024-06-12 15:49_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 15:49_

Yeah I'm going to use your example above to write a regression test.

---

_@BurntSushi reviewed on 2024-06-12 17:01_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:196 on 2024-06-12 17:01_

All righty, this should be fixed now! I added a regression test here: https://github.com/astral-sh/packse/pull/193

---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-12 17:11_

---

_@charliermarsh reviewed on 2024-06-12 17:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:226 on 2024-06-12 17:14_

Why can this one unwrap?

---

_@charliermarsh approved on 2024-06-12 17:14_

---

_@BurntSushi reviewed on 2024-06-12 17:20_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/requires_python.rs`:226 on 2024-06-12 17:20_

Same reason as the one above. I duplicated the comment to make this clearer.

Although I suppose the constructor of `RequiresPython` permits any `Version` which means it could technically have a local segment. Likely a logic error, but I added `without_local()` to strip it from the version anyway.

---

_Merged by @BurntSushi on 2024-06-12 17:30_

---

_Closed by @BurntSushi on 2024-06-12 17:30_

---

_Branch deleted on 2024-06-12 17:30_

---
