```yaml
number: 15834
title: "[red-knot] gather type prevalence statistics"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: coverage
created_at: 2025-01-30T18:48:46Z
updated_at: 2025-02-01T18:21:52Z
url: https://github.com/astral-sh/ruff/pull/15834
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] gather type prevalence statistics

---

_Pull request opened by @carljm on 2025-01-30 18:48_

Something Alex and I threw together during our 1:1 this morning. Allows us to collect statistics on the prevalence of various types in a file, most usefully TODO types or other dynamic types.

---

_Label `red-knot` added by @carljm on 2025-01-30 18:48_

---

_Review requested from @MichaReiser by @carljm on 2025-01-30 18:48_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-30 18:48_

---

_Review requested from @sharkdp by @carljm on 2025-01-30 18:48_

---

_Comment by @carljm on 2025-01-30 19:01_

Hmm, the test failure here is very odd! Test definitely passes locally. Will have to look into this later.

---

_Comment by @AlexWaygood on 2025-01-30 21:44_

that is really weird. it passes for me locally too (and it's obviously passing on all but one of our CI jobs).

That said, the statistic given by the one job that's failing is a statistic that makes more sense intuitively to me...

---

_Comment by @carljm on 2025-01-31 00:04_

The issue is that in a release build we (intentionally, for perf reasons) omit tracking of distinct messages / file-and-line information for Todo types. So this results in fewer different kinds of Todo types in a release build. Then this interacts with another bug, which is that we are using `FxHashMap::extend` wrongly to combine statistics from different scopes (this won't sum the totals when the key exists in both maps). The latter bug only shows up if the same type occurs in more than one scope, and that only occurs in our tests when running in release mode so that all Todo types are the same.

---

_Comment by @carljm on 2025-01-31 00:32_

Pushed a commit that fixes statistics merging, and fixes the tests.

It remains the case that we can't add any tests specifically testing that the statistics for "different" Todo types are differentiated, or those tests will fail on a release build. (We could switch any such tests off in release build.) We can still build features making use of this differentiation, but they will only work in debug build.

---

_Comment by @AlexWaygood on 2025-01-31 10:32_

> We can still build features making use of this differentiation, but they will only work in debug build.

I think it's okay to make some statistics-related features only available if debug assertions are enabled. Statistics collection doesn't need to be that fast.

---

_@AlexWaygood approved on 2025-01-31 10:42_

The bug we had in our use of `HashMap::extend()` makes me wonder if it is worth using a "proper" reimplementation of Python's `collections.Counter` class, like https://docs.rs/counter/latest/counter/, rather than rolling our own version. We could make the additional dependency conditional on a `statistics` optional feature.

But for now, it still may not be worth the additional dependency.

---

_Merged by @carljm on 2025-01-31 15:10_

---

_Closed by @carljm on 2025-01-31 15:10_

---

_Branch deleted on 2025-01-31 15:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/statistics.rs`:7 on 2025-01-31 21:43_

Do we expect this method to be called multiple times in the same revision and file for it to be a salsa query? 

---

_@MichaReiser reviewed on 2025-01-31 21:43_

---

_@MichaReiser reviewed on 2025-01-31 21:45_

Should this be gated to debug only builds? How is it intended to be used?

---

_Comment by @MichaReiser on 2025-01-31 22:22_

Depending on what this is used for: have you considered using Countme? E.g. by adding a count flag to every relevant type?

---

_@carljm reviewed on 2025-01-31 22:52_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/statistics.rs`:7 on 2025-01-31 22:52_

Perhaps roughly equally likely as `check_types`, which is a query? It's very parallel to `check_types`, so seems reasonable to handle them the same way.

---

_Comment by @carljm on 2025-01-31 22:57_

> Should this be gated to debug only builds? How is it intended to be used?

Two potential uses are as part of ecosystem check, and as a user-facing feature to measure static type coverage. Former could perhaps be debug-gated (though we may want to run ecosystem check on release build for speed), latter cannot be.

> have you considered using Countme?

I don't think countme works for this. If we have 20 expressions in a file typed as `Type::Any`, there aren't 20 distinct instances of any struct created that could be counted with countme.



---

_Comment by @MichaReiser on 2025-02-01 08:51_

I'm sort of leaning towards reverting this change until we have a concrete usage that informs the design and avoids this just being dead code:

* It's unclear if the function should be a query (is this function ever called in watch mode?)
* The map currently counts `Literal["str"]` and `Literal["st"]` as two separate types. Do we need this precision? If not, we could use a `Vec` instead of a hash map with absolute indices for the few types we want to distinguish (e.g. how many classes, functions, string literals, etc)
* What about types nested in `Union`s and `Intersection`s or declared types? The current implementation disregards them completely

---

_Comment by @AlexWaygood on 2025-02-01 17:14_

The idea was just to land an MVP that wasn't too complicated. Perhaps you're right and this really is too minimal to even be worth keeping in the source code, though.

> It's unclear if the function should be a query (is this function ever called in watch mode?)

probably not

> The map currently counts `Literal["str"]` and `Literal["st"]` as two separate types. Do we need this precision? If not, we could use a `Vec` instead of a hash map with absolute indices for the few types we want to distinguish (e.g. how many classes, functions, string literals, etc)

What's the downside of having this level of precision?

> What about types nested in `Union`s and `Intersection`s or declared types? The current implementation disregards them completely

We're aware. Again, this was meant to be just a simple MVP. I think even just knowing the number of "pure TODO types" we infer when running on `tomllib` would be interesting and useful to me right now. And since each TODO type records its provenance when we run red-knot with debug assertions enabled, this could give us useful information about which features should be prioritised next (which TODOs are most important to tackle).

---

_Comment by @MichaReiser on 2025-02-01 18:21_

> What's the downside of having this level of precision?

More granular statistics have a higher runtime cost (performance and storage). The most simple representation -- and probably sufficient for this MVP -- is to only count the number of todo types. This can be done with a single usize. The next step is to store the type kind: Literal, Unknown, any. This can be done with a fixed array or vector. Counting the unique usages of each type (which this PR implements) requires a hash map which requires expensive hashing and is probably large. We should only use the hash map solution if we have a need for it -- which I don't think we currently do

---
