```yaml
number: 1298
title: Refactor incompatiblity tracking for distributions
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/incompatible-dists
created_at: 2024-02-14T17:37:51Z
updated_at: 2024-03-08T17:02:32Z
url: https://github.com/astral-sh/uv/pull/1298
synced_at: 2026-01-10T14:54:43Z
```

# Refactor incompatiblity tracking for distributions

---

_Pull request opened by @zanieb on 2024-02-14 17:37_

Extends the "compatibility" types introduced in #1293 to apply to source distributions as well as wheels.

- We now track the most-relevant incompatible source distribution
- Exclude newer, Python requirements, and yanked versions are all tracked as incompatibilities in the new model (this lets us remove `DistMetadata`!)

---

_Review comment by @zanieb on `crates/puffin/tests/pip_compile.rs`:224 on 2024-02-14 17:46_

This is really obnoxious...


---

_@zanieb reviewed on 2024-02-14 17:46_

---

_Label `internal` added by @zanieb on 2024-02-14 18:59_

---

_Comment by @zanieb on 2024-02-19 17:47_

This needs a serious rebase.

Still important work but prioritizing bug fixes first.

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:345 on 2024-03-07 19:17_

This ensures we do not regress as described in https://github.com/astral-sh/uv/pull/2066#discussion_r1506843879

---

_@zanieb reviewed on 2024-03-07 19:17_

---

_@zanieb reviewed on 2024-03-07 19:17_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:390 on 2024-03-07 19:17_

Can `RequiresPython` be refactored to avoid this?

---

_@zanieb reviewed on 2024-03-07 19:20_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:72 on 2024-03-07 19:20_

These three overlap with variants in `IncompatibleWheel`. Should we combine them into a structure like:

```
Incompatible
    ExcludeNewer
    RequiresPython
    Yanked
    Wheel
        NoBinary
        Tag
    SourceDist
        NoBuild
```


---

_Review requested from @charliermarsh by @zanieb on 2024-03-07 19:28_

---

_Comment by @zanieb on 2024-03-07 19:29_

I think this can be improved further, but now that it includes the user-facing improvement from #2066 I think we may want to bias towards merging.

Happy for any suggestions on different structuring.

---

_Marked ready for review by @zanieb on 2024-03-07 19:30_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:253 on 2024-03-08 01:22_

Why not `Ord`?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 01:23_

Why not `Ord`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:260 on 2024-03-08 01:26_

Nit: incomplete message here.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:345 on 2024-03-08 01:28_

The formatting here seems off.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:390 on 2024-03-08 01:30_

You could consider putting this whole block in here instead of using the `continue` and the `unreachable!()`:

```rust
let python_version = requires_python
    .iter()
    .map(PubGrubSpecifier::try_from)
    .fold_ok(Range::full(), |range, specifier| {
        range.intersection(&specifier.into())
    })?;

let package = &next;
for kind in [PubGrubPython::Installed, PubGrubPython::Target] {
    state.add_incompatibility(Incompatibility::from_dependency(
        package.clone(),
        Range::singleton(version.clone()),
        (PubGrubPackage::Python(kind), python_version.clone()),
    ));
}
state.partial_solution.add_decision(next.clone(), version);
continue;
```

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/version_map.rs`:390 on 2024-03-08 01:32_

I think the ordering of the incompatibility checks is different between this branch and the `DistFilename::SourceDistFilename` branch. Is that intentional?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/version_map.rs`:397 on 2024-03-08 01:34_

Maybe collapse into `if yanked.is_some_and(|yanked| yanked.is_yanked())`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/version_map.rs`:116 on 2024-03-08 01:35_

Maybe we should only include the versions here? `AllowedYanks` is `FxHashMap<PackageName, FxHashSet<Version>>` but we only need the versions since we're looking at a single package name.

---

_@charliermarsh reviewed on 2024-03-08 01:36_

I think this is a big improvement.

---

_@charliermarsh reviewed on 2024-03-08 01:37_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 01:37_

I think my branch just made everything `Ord` and then I had, like, `compatibility > existing` or something. But maybe it ends up being more complicated, the logic in here is quite nuanced. (This isn't blocking feedback.)

---

_@zanieb reviewed on 2024-03-08 02:46_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 02:46_

Satisfying `Ord` is actually really challenging. For example, when comparing `RequiresPython` incompatibilities that contain version specifiers we cannot really provide strong ordering. I attempted to do `Ord` at first as well and after much discussion @BurntSushi recommended this approach instead.

I believe the reason this worked on your branch is that you didn't retain information on the incompatibility enum members. Enum members themselves are trivially ordered, but once we store additional metadata about the incompatibility in them we need ordering in a larger hierarchy of types.

If you have other ideas to get around this please share :)

---

_@zanieb reviewed on 2024-03-08 03:18_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 03:18_

Separately.. the nuance here is a little frightening to me. Selecting the "most relevant incompatible distribution" feels like it could be brittle.

---

_Review comment by @konstin on `crates/distribution-types/src/prioritized_distribution.rs`:280 on 2024-03-08 08:50_

Is that still deterministic, e.g. if the order we get from the index changes?

---

_Review comment by @konstin on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 08:53_

If there's no clear precedence, can with just stringifying the incompatibility criteria and sort based on that? It's arbitrary yet deterministic and easy to follow.

---

_Review comment by @konstin on `crates/uv-resolver/src/version_map.rs`:37 on 2024-03-08 08:57_

I hope no index implements this, this would be quite confusing to users

---

_@konstin approved on 2024-03-08 08:58_

This is a big improvement for the error messages

---

_@zanieb reviewed on 2024-03-08 15:02_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:280 on 2024-03-08 15:02_

It's not a change from the current status-quo where we take the "first" compatible source distribution. I think we sort the output from the index immediately though (as a guard against non-determinism).

---

_@zanieb reviewed on 2024-03-08 15:05_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 15:05_

That's actually what I did to implement `Ord` originally, but it's hard to be sure we're meeting all of the requirements for the trait and it seems brittle.

---

_@notatallshaw reviewed on 2024-03-08 15:45_

---

_Review comment by @notatallshaw on `crates/uv-resolver/src/version_map.rs`:37 on 2024-03-08 15:45_

Fun fact, the spec is about marking a particular file as yanked: https://peps.python.org/pep-0592/.

PyPI has implemented it as only being able to yank an entire release, but that is a PyPI implementation detail.

This has come up a few times on discussing particular behavior pip could adopt around yanking.

---

_Review comment by @zanieb on `crates/uv-resolver/src/version_map.rs`:397 on 2024-03-08 15:57_

We need the unwrapped option down a few lines.

---

_@zanieb reviewed on 2024-03-08 15:57_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-08 15:57_

I think what you have here is totally fine.

---

_@charliermarsh reviewed on 2024-03-08 15:57_

---

_@konstin reviewed on 2024-03-08 16:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/version_map.rs`:37 on 2024-03-08 16:35_

I see why it needs to be like that on the API level (there are no releases in the API, just files), but i'd strongly prefer it if indexes made this an all or nothing operation per release.

> While this PEP implements yanking at the file level, that is largely due to the shape the simple repository API takes, not a specific decision made by this PEP.

---

_@zanieb reviewed on 2024-03-08 16:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:390 on 2024-03-08 16:48_

Then we need to write that whole thing twice which I find less ideal.

My concern is less about the `unreachable!` and more that it suggests a flaw in the abstraction.

---

_@charliermarsh reviewed on 2024-03-08 16:53_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:390 on 2024-03-08 16:53_

Yeah, the abstraction is awkward, but I guess what I was pointing out (which maybe isn't useful) is that to the degree that the `unreachable!` itself represents a flaw in the abstraction, that's just a consequence of how the code is structured locally.

---

_@notatallshaw reviewed on 2024-03-08 16:56_

---

_Review comment by @notatallshaw on `crates/uv-resolver/src/version_map.rs`:37 on 2024-03-08 16:56_

Agreed, there has been some recent discussion about requiring behavior on indexes, not just the API, and the concern was being able to find and get buy in from non PyPI indexes (Azure, Gitlab, Artifactory, etc.). Perhaps a good case to put forth as a trial to see if it's possible.

---

_@zanieb reviewed on 2024-03-08 16:56_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:390 on 2024-03-08 16:56_

👍 yeah that makes sense. This would be partially alleviated by a restructuring to avoid duplication as described in https://github.com/astral-sh/uv/pull/1298#discussion_r1516699939

---

_Merged by @zanieb on 2024-03-08 17:02_

---

_Closed by @zanieb on 2024-03-08 17:02_

---

_Branch deleted on 2024-03-08 17:02_

---
