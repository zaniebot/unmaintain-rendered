```yaml
number: 5099
title: "Handle universal vs. fork markers with `ResolverMarkers`"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: konsti/better-marker-reporting
created_at: 2024-07-16T09:57:30Z
updated_at: 2024-07-17T16:59:35Z
url: https://github.com/astral-sh/uv/pull/5099
synced_at: 2026-01-10T13:42:52Z
```

# Handle universal vs. fork markers with `ResolverMarkers`

---

_Pull request opened by @konstin on 2024-07-16 09:57_

* Use a dedicated `ResolverMarkers` check in the fork state. This is better than the `MarkerTree::And(Vec::new())` check.
* Report the timing correct naming universal resolution instead of two spaces around an empty string when there are no markers.
* On resolution error, show the split that we're in. I'm not sure how to word this, since we're doing a universal resolution until we fork, so the trace may contain information from requirements that are not part of this fork.

---

_Label `error messages` added by @konstin on 2024-07-16 09:57_

---

_Label `preview` added by @konstin on 2024-07-16 09:57_

---

_Review requested from @BurntSushi by @konstin on 2024-07-16 09:57_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1488 on 2024-07-16 12:37_

Hmmm. So I very intentionally used `MarkerTree` instead of `Option<MarkerTree>`. For example, in this routine, I don't think there is any semantic difference between `None` and `Some(MarkerTree::And(vec![]))`. They are redundant with one another, which I find confusing, because it injects an extra state into the type system that you need to think about. Moreover, there _are_ perhaps cases where "a marker that is always true" and a "possibly absent marker" are semantically distinct, and IMO that's when we should use `Option<MarkerTree>`.

---

_@BurntSushi approved on 2024-07-16 12:48_

The improvements here look good.

With respect to `MarkerTree` and `Option<MarkerTree>`, my idea here was to collapse states that weren't semantically distinct. I now see that the `is_universal` check was added later as a way of determining whether the resolver was in "universal" mode or not. (And this is doubly confusing because the resolver is in universal mode when `MarkerTree::is_universal` is _false_.) The idea was that the canonical way of determining whether the resolver was in universal mode or not is whether the [resolver has a marker environment](https://github.com/astral-sh/uv/blob/545bbf92863f4109070ebeb922c837b430441b76/crates/uv-resolver/src/resolver/mod.rs#L98-L99), and _not_ whether there's a marker expression that is trivially true or not. So for example, this code:

https://github.com/astral-sh/uv/blob/545bbf92863f4109070ebeb922c837b430441b76/crates/uv-resolver/src/resolver/mod.rs#L309-L318

Should I think be using `self.markers` (which, confusingly, is a `MarkerEnvironment`) to determine whether to emit log messages about a split.

This one can too:

https://github.com/astral-sh/uv/blob/545bbf92863f4109070ebeb922c837b430441b76/crates/uv-resolver/src/resolver/mod.rs#L585-L590

But this one is trickier:

https://github.com/astral-sh/uv/blob/545bbf92863f4109070ebeb922c837b430441b76/crates/uv-resolver/src/fork_urls.rs#L42-L47

And I think it's that case that caused `MarkerTree::is_universal` to come into existence. And I agree that `is_universal` is Not Good, and that given the choice between "checking for a single trivially true marker expression" and "using an option," the latter is better. But I'm worried about `Option<MarkerTree>` carrying subtly different semantics for the `None` case depending on where we are in the code. But maybe we don't yet have the right abstractions to deal with this nicely.

---

_@konstin reviewed on 2024-07-16 13:23_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1488 on 2024-07-16 13:23_

Do we ever get a fork with `Some(MarkerTree::And(vec![]))`? It seems to me that we shouldn't fork in that case since we're still doing a universal resolution.

---

_@BurntSushi reviewed on 2024-07-16 13:45_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1488 on 2024-07-16 13:45_

I don't believe so, but that invariant isn't encoded into the type system. It's more like it's manifest in the concept of forking itself. It's not something you can easily employ local reasoning about when looking at just the types.

I think this is a fine change for now. I think this change is probably better than the status quo. :-)

---

_Renamed from "Slightly better split marker reporting" to "Handle unviersal vs. fork markers with `ResolverMarkers`" by @konstin on 2024-07-16 21:26_

---

_Renamed from "Handle unviersal vs. fork markers with `ResolverMarkers`" to "Handle universal vs. fork markers with `ResolverMarkers`" by @konstin on 2024-07-16 21:28_

---

_@konstin reviewed on 2024-07-16 21:30_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1488 on 2024-07-16 21:30_

I encoded it in the type system using `ResolverMarkers`. Probably needs a fresh read tomorrow morning, but the api feels a good bit better this way.

---

_Review requested from @BurntSushi by @konstin on 2024-07-17 12:33_

---

_@BurntSushi reviewed on 2024-07-17 13:26_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2891 on 2024-07-17 13:26_

Hmmm. So I think the confusing thing about this name is that, if I'm understanding correctly, `ResolverMarkers::Universal` is used when the resolver is _not_ in universal mode. It has the right semantics, but the name is I think confusable.

I'm actually finding it quite difficult to come up with a good name here. I think the problem is in using the markers (and not the marker environment) to identify whether we're employing a possibly-forking strategy or not.

Maybe... `ResolverMarkers::AlwaysTrue`? ¯\\\_(ツ)_/¯

---

_@BurntSushi reviewed on 2024-07-17 13:27_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1532 on 2024-07-17 13:27_

If we have a newtype for this, I think I'd rather see these case analyses collapsed. Perhaps a method like `ResolverMarkers::possible_to_satisfy` or something.

---

_@konstin reviewed on 2024-07-17 16:24_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2891 on 2024-07-17 16:24_

The markers represent a trinary state - specific environment, universal resolution, resolution in a fork - so i changed the type to express that too.

---

_@BurntSushi approved on 2024-07-17 16:52_

Yeah I think this is better. I think we can still collapse/encapsulate case analyses, but that can be a follow-up.

---

_Merged by @konstin on 2024-07-17 16:59_

---

_Closed by @konstin on 2024-07-17 16:59_

---

_Branch deleted on 2024-07-17 16:59_

---
