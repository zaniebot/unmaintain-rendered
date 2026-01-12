```yaml
number: 3595
title: "uv-distribution: include all wheels in distribution types"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/propagate-wheels
created_at: 2024-05-14T22:45:10Z
updated_at: 2024-05-15T19:07:30Z
url: https://github.com/astral-sh/uv/pull/3595
synced_at: 2026-01-12T16:05:44Z
```

# uv-distribution: include all wheels in distribution types

---

_@BurntSushi_

Our current flow of data from "simple registry package" to "final
resolved distribution" goes through a number of types:

* `SimpleMetadata` is the API response from a registry that includes all
published versions for a package. Each version has an assortment of metadata
associated with it.
* `VersionFiles` is the aforementioned metadata. It is split in two: a
group of files for source distributions and a group of files for wheels.
* `PrioritizedDist` collects a subset of the files from `VersionFiles`
to form a selection of the "best" sdist and the "best" wheel for the
current environment.
* `CompatibleDist` is created from a borrowed `PrioritizedDist` that,
perhaps among other things, encapsulates the decision of whether to pick
an sdist or a wheel. (This decision depends both on compatibility and
the action being performed. e.g., When doing installation, a
`CompatibleDist` will sometimes select an sdist over a wheel.)
* `ResolvedDistRef` is like a `ResolvedDist`, but borrows a `Dist`.
* `ResolvedDist` is the almost-final-form of a distribution in a
resolution and is created from a `ResolvedDistRef`.
* `AnnotatedResolvedDist` is a new data type that is the actual final
form of a distribution that a universal lock file cares about. It
bundles a `ResolvedDist` with some metadata needed to generate a lock
file.

One of the requirements of a universal lock file is that we include all
wheels (and maybe all source distributions? but at least one if it's
present) associated with a distribution. But the above flow of data (in
the step from `VersionFiles` to `PrioritizedDist`) drops all wheels
except for the best one.

To remedy this, in this PR, we rejigger `PrioritizedDist`,
`CompatibleDist` and `ResolvedDistRef` so that all wheel data is
preserved. And when a `ResolvedDistRef` is finally turned into a
`ResolvedDist`, we copy all of the wheel data. And finally, we adjust
the `Lock` constructor to read this new data and include it in the lock
file. To make this work, we also modify `RegistryBuiltDist` so that it
can contain one or more wheels instead of just one.

One shortcoming here (called out in the code as a FIXME) is that if a
source distribution is selected as the "best" thing to use (perhaps
there are no compatible wheels), then the wheels won't end up in the
lock file. I plan to fix this in a follow-up PR.

We also aren't totally consistent on source distribution naming.
Sometimes we use `sdist`. Sometimes `source`. Sometimes `source_dist`.
I think it'd be nice to just use `sdist` everywhere, but I do prefer
the type names to be `SourceDist`. And sometimes you want function
names to match the type names (i.e., `from_source_dist`), which in turn
leads to an appearance of inconsistency. I'm open to ideas.

Closes #3351

---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-15 15:29_

---

_Comment by @codspeed-hq[bot] on 2024-05-15 15:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/propagate-wheels)

### Merging #3595 will **degrade performances by 10.28%**

<sub>Comparing <code>ag/propagate-wheels</code> (7308f17) with <code>main</code> (8d68d45)</sub>



### Summary

`❌ 1` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ag/propagate-wheels)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ag/propagate-wheels` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 371 ms | 413.5 ms | -10.28% |


---

_Review requested from @konstin by @BurntSushi on 2024-05-15 15:29_

---

_Marked ready for review by @BurntSushi on 2024-05-15 15:29_

---

_Comment by @charliermarsh on 2024-05-15 15:32_

Do you think that's a real regression?

---

_Comment by @BurntSushi on 2024-05-15 15:46_

> Do you think that's a real regression?

It's possible. The regression is highlighting the drop impl for `ResolvedDist`, which is definitely an area that has been touched here.

The various distribution data is cloned occasionally in full. I don't have a complete picture yet on how to eliminate those clones, but we probably should, especially since we're carrying around more data now. It's _possible_ that fixing this is a gnarly refactor though.

I'll take a look and see if there's a quicker fix.

---

_Comment by @BurntSushi on 2024-05-15 15:55_

I can't measure a regression locally. This PR actually seems to be consistently a hair faster:

```
$ hyperfine -w10 -r1000 'uv-main pip compile ~/astral/tmp/reqs/jupyter.in' 'uv pip compile ~/astral/tmp/reqs/jupyter.in'
Benchmark 1: uv-main pip compile ~/astral/tmp/reqs/jupyter.in
  Time (mean ± σ):      33.9 ms ±   8.8 ms    [User: 28.5 ms, System: 44.0 ms]
  Range (min … max):    19.2 ms …  75.0 ms    1000 runs

Benchmark 2: uv pip compile ~/astral/tmp/reqs/jupyter.in
  Time (mean ± σ):      32.8 ms ±   8.6 ms    [User: 27.4 ms, System: 42.6 ms]
  Range (min … max):    19.9 ms …  73.2 ms    1000 runs

Summary
  uv pip compile ~/astral/tmp/reqs/jupyter.in ran
    1.04 ± 0.38 times faster than uv-main pip compile ~/astral/tmp/reqs/jupyter.in
```

---

_Comment by @charliermarsh on 2024-05-15 15:58_

Okay, that's fine with me then.

---

_Comment by @charliermarsh on 2024-05-15 15:59_

Thanks for investigating.

---

_Comment by @BurntSushi on 2024-05-15 16:05_

Aye. And looking more at our distribution types to see if we can avoid copying them, it looks a little tricky. I think the main problem is that `VersionMap` is the owner of `PrioritizedDist` and `PrioritizedDist` is ultimately the owner of each `RegistryBuiltDist` (which is now a list of wheels and an optional sdist). When we do the `PrioritizedDist -> CandidateDist -> ResolvedDistRef -> ResolvedDist` sequence, it's the `ResolvedDistRef -> ResolvedDist` conversion that does the copy. The `VersionMap` data structure isn't really designed to give up ownership, and I don't see an obvious way to do it. That in turn probably means that the key to avoiding a copy is reference counting. But that means injecting an `Arc` into types like `RegistryBuiltDist`. I'm not quite sure that's worth doing.

Anyway, I agree. I also saw mention on Discord that `resolve_warm_jupyter` was noisy in particular. So I think I'm also okay with plowing ahead for now.

---

_Comment by @BurntSushi on 2024-05-15 16:30_

> One shortcoming here (called out in the code as a FIXME) is that if a
> source distribution is selected as the "best" thing to use (perhaps
> there are no compatible wheels), then the wheels won't end up in the
> lock file. I plan to fix this in a follow-up PR.

I did this in #3610.

(Also, this PR and #3610 are ready for review.)

---

_Comment by @charliermarsh on 2024-05-15 16:55_

You got it, I will review today.

---

_@charliermarsh reviewed on 2024-05-15 17:54_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:228 on 2024-05-15 17:54_

Nit: remove

---

_@charliermarsh reviewed on 2024-05-15 17:56_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolved.rs`:62 on 2024-05-15 17:56_

What's the difference between `sdist` and `source` here?

---

_@charliermarsh reviewed on 2024-05-15 17:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:784 on 2024-05-15 17:57_

It looks like this got changed back from `let hash = reg_dist.file.hashes.first().cloned().map(Hash::from);`. Is that intentional?

---

_@charliermarsh reviewed on 2024-05-15 17:58_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:446 on 2024-05-15 17:58_

Where is this used? It seems somewhat lossy, is it dangerous that it exists?

---

_@charliermarsh reviewed on 2024-05-15 17:58_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:538 on 2024-05-15 17:58_

Can this be `self.best_wheel().name()`?

---

_@charliermarsh reviewed on 2024-05-15 17:58_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:623 on 2024-05-15 17:58_

Can this be `self.best_wheel().version_or_url()`, to avoid re-encoding the implementation?

---

_@charliermarsh reviewed on 2024-05-15 18:00_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolved.rs`:62 on 2024-05-15 18:00_

Looks like no difference, but `wheel` and `built` _do_ differ in the `Self::InstallableRegistryBuiltDist` case.

---

_@charliermarsh approved on 2024-05-15 18:01_

---

_@charliermarsh reviewed on 2024-05-15 18:02_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolved.rs`:62 on 2024-05-15 18:02_

I see, in the next PR they _do_ differ.

---

_@BurntSushi reviewed on 2024-05-15 18:34_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/resolved.rs`:62 on 2024-05-15 18:34_

Yeah it's a bit weird. My sense is that the types could possibly be clearer here if we could find a way to split `PrioritizedDist` into a "builder phase" and "final phase." But I'm not sure.

---

_@BurntSushi reviewed on 2024-05-15 18:36_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:784 on 2024-05-15 18:36_

Ah no, not intentional. Maybe a bad merge. Or I fumbled conflict resolution.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/lock.rs`:784 on 2024-05-15 18:36_

I switched it back.

---

_@BurntSushi reviewed on 2024-05-15 18:36_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/lib.rs`:446 on 2024-05-15 18:48_

Yeah this is a really nice catch. I had added this to help with the refactor, but it turned out it was still being used to convert a `ResolvedDistRef` into a `Request` in the resolver. This was indeed dropping all of the wheels that were being carried around, although I don't know much it matters. (It's also another copy of the dist data.)

I've removed this `From` impl and added a `From<ResolvedDistRef> for Request` impl that does the conversion correctly.

---

_@BurntSushi reviewed on 2024-05-15 18:48_

---

_@BurntSushi reviewed on 2024-05-15 18:49_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/lib.rs`:623 on 2024-05-15 18:49_

Yup. I've fixed the other instances of this too.

---

_Comment by @BurntSushi on 2024-05-15 19:07_

The perf regression remains (and actually got bigger), but https://github.com/astral-sh/uv/pull/3610, which builds on this one, has no regression. And there's nothing in #3610 that ought to have impacted things here. I re-ran my hyperfine benchmark from above to double check:

```
[andrew@duff uv]$ hyperfine -w10 -r1000 'uv-main pip compile ~/astral/tmp/reqs/jupyter.in' 'uv pip compile ~/astral/tmp/reqs/jupyter.in' ; A bart
Benchmark 1: uv-main pip compile ~/astral/tmp/reqs/jupyter.in
  Time (mean ± σ):      32.4 ms ±   8.7 ms    [User: 26.9 ms, System: 43.4 ms]
  Range (min … max):    19.1 ms …  66.8 ms    1000 runs

Benchmark 2: uv pip compile ~/astral/tmp/reqs/jupyter.in
  Time (mean ± σ):      32.5 ms ±   8.1 ms    [User: 27.7 ms, System: 42.6 ms]
  Range (min … max):    19.8 ms …  66.4 ms    1000 runs

Summary
  uv-main pip compile ~/astral/tmp/reqs/jupyter.in ran
    1.00 ± 0.37 times faster than uv pip compile ~/astral/tmp/reqs/jupyter.in
```

So I'm going to go ahead and merge this. It looks like we might have a noisy benchmark.

---

_Merged by @BurntSushi on 2024-05-15 19:07_

---

_Closed by @BurntSushi on 2024-05-15 19:07_

---

_Branch deleted on 2024-05-15 19:07_

---
