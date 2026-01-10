```yaml
number: 2596
title: Consider installed packages during resolution
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/installed-resolve
created_at: 2024-03-21T18:47:07Z
updated_at: 2024-03-28T18:49:18Z
url: https://github.com/astral-sh/uv/pull/2596
synced_at: 2026-01-10T14:49:08Z
```

# Consider installed packages during resolution

---

_Pull request opened by @zanieb on 2024-03-21 18:47_

Previously, we did not consider installed distributions as candidates while performing resolution. Here, we update the resolver to use installed distributions that satisfy requirements instead of pulling new distributions from the registry. 

The implementation details are as follows:

- We now provide `SitePackages` to the `CandidateSelector`
- If an installed distribution satisfies the requirement, we prefer it over remote distributions
- We do not want to allow installed distributions in some cases, i.e., upgrade and reinstall
    - We address this by introducing an `Exclusions` type which tracks installed packages to ignore during selection
- There's a new `ResolvedDist` wrapper with `Installed(InstalledDist)` and `Installable(Dist)` variants
    - This lets us pass already installed distributions throughout the resolver

The user-facing behavior is thoroughly covered in the tests, but briefly:

- Installing a package that depends on an already-installed package prefers the local version over the index
- Installing a package with a name that matches an already-installed URL package does not reinstall from the index
- Reinstalling (--reinstall) a package by name _will_ pull from the index even if an already-installed URL package is present
   - To reinstall the URL package, you must specify the URL in the request

Closes https://github.com/astral-sh/uv/issues/1661

Addresses:

- https://github.com/astral-sh/uv/issues/1476
- https://github.com/astral-sh/uv/issues/1856
- https://github.com/astral-sh/uv/issues/2093
- https://github.com/astral-sh/uv/issues/2282
- https://github.com/astral-sh/uv/issues/2383
- https://github.com/astral-sh/uv/issues/2560

## Test plan

- [x] Reproduction at `charlesnicholson/uv-pep420-bug` passes
- [x] Unit test for editable package ([#1476](https://github.com/astral-sh/uv/issues/1476))
- [x] Unit test for previously installed package with empty registry
- [x] Unit test for local non-editable package
- [x] Unit test for new version available locally but not in registry ([#2093](https://github.com/astral-sh/uv/issues/2093))
- ~[ ] Unit test for wheel not available in registry but already installed locally ([#2282](https://github.com/astral-sh/uv/issues/2282))~ (seems complicated and not worthwhile)
- [x] Unit test for install from URL dependency then with matching version ([#2383](https://github.com/astral-sh/uv/issues/2383))
- [x] Unit test for install of new package that depends on installed package does not change version ([#2560](https://github.com/astral-sh/uv/issues/2560))
- [x] Unit test that `pip compile` does _not_ consider installed packages

---

_@zanieb reviewed on 2024-03-21 18:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:87 on 2024-03-21 18:48_

@charliermarsh is it obvious to you how to do this?

---

_@zanieb reviewed on 2024-03-21 18:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 18:48_

I feel like doing this _before_ the `VersionMap` may be incorrect. I'll need to understand the goal here better.

---

_@charliermarsh reviewed on 2024-03-21 18:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 18:51_

I think doing it before the version map is right, but it also needs to work with `preferences` above, since these will _always_ be in preferences. Maybe you can leverage that? Maybe this should _only_ be in the `if` block above?

---

_@zanieb reviewed on 2024-03-21 18:52_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 18:52_

👍 I can do that, although I'm not sure why that's helpful to combine except perhaps performance?

---

_@zanieb reviewed on 2024-03-21 19:12_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:87 on 2024-03-21 19:12_

Technically I just need a `Dist`

---

_@charliermarsh reviewed on 2024-03-21 19:13_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:87 on 2024-03-21 19:13_

Not exactly, no... It's possible that we want an `enum` for `Candidate`, to represent an already-installed dist?

---

_@zanieb reviewed on 2024-03-21 19:13_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:87 on 2024-03-21 19:13_

Like am I supposed to _just_ turn `InstalledDirectUrlDist` into `DirectUrlSourceDist` or something?

---

_@zanieb reviewed on 2024-03-21 19:14_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:87 on 2024-03-21 19:14_

Hm I didn't consider that... I'll explore that idea

---

_@zanieb reviewed on 2024-03-21 19:15_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 19:15_

We sometimes intentionally omit `preferences` right so wouldn't these sometimes _not_ be present there?

---

_@charliermarsh reviewed on 2024-03-21 19:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 19:19_

Yeah I guess that’s true.

---

_@zanieb reviewed on 2024-03-21 22:00_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:84 on 2024-03-21 22:00_

This is complicated... editables are not present in preferences. We'll need to be careful with this, I think.

---

_@zanieb reviewed on 2024-03-21 22:03_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:28 on 2024-03-21 22:03_

Clippy is upset about the size of this variant and it would make sense for it to be borrowed like the rest, but I'm not sure how to create a borrowed distribution in `CandidateSelector`.

---

_@zanieb reviewed on 2024-03-21 22:04_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1002 on 2024-03-21 22:04_

There is a sprinkling of this... perhaps suggesting the wrong abstraction.

---

_@charliermarsh reviewed on 2024-03-22 17:10_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:122 on 2024-03-22 17:10_

I think adding this to `Dist` is not quite right... I feel like we need some other enum on top of this, like `ResolvedDist` that can either be a remote distribution (`Dist`) or an installed distribution.

---

_@charliermarsh reviewed on 2024-03-22 17:11_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:327 on 2024-03-22 17:11_

E.g., I think this should probably happen at a level above? Passing an installed distribution to this remote fetcher feels off.

---

_@zanieb reviewed on 2024-03-22 18:40_

---

_Review comment by @zanieb on `crates/distribution-types/src/lib.rs`:122 on 2024-03-22 18:40_

Yeah I think I share your feeling. Bigger change but might be a better level...

---

_@zanieb reviewed on 2024-03-25 21:08_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-25 21:08_

Is there a canonical name for this?

---

_@zanieb reviewed on 2024-03-25 21:09_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-25 21:09_

This is basically the whole reason `ResolvedDistRef` exists. Maybe we should just clone here? We clone `Dist` all over the place already...

---

_@zanieb reviewed on 2024-03-25 22:06_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-25 22:06_

Perhaps `cloned()`?

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3229 on 2024-03-25 22:45_

These filters need clean-up, copied from Charlie's test case but it's not a temporary directory it's the workspace

---

_@zanieb reviewed on 2024-03-25 22:45_

---

_@zanieb reviewed on 2024-03-25 22:46_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3131 on 2024-03-25 22:46_

I'm not sure we actually need this test

---

_@zanieb reviewed on 2024-03-25 22:46_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3214 on 2024-03-25 22:46_

You'll notice all the reinstalls panic... this needs to be addressed.

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolution.rs`:151 on 2024-03-26 01:00_

Does this need `clone()`? It looks like it already takes an owned value?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 01:02_

In the `Cow` impl it's called `into_owned` but `into` usually consumes the value. I'd say `to_owned`: https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:95 on 2024-03-26 01:07_

I wonder about the value of making this generic. It means we need separate implementations for each concrete type when compiling. I know it's used for the empty package, but could that just be represented by an empty `SitePackages`?

---

_@charliermarsh reviewed on 2024-03-26 01:10_

I think this generally looks good!

---

_@zanieb reviewed on 2024-03-26 01:40_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 01:40_

Hm I initially did `to_owned` but got confused about how that interacts with the `ToOwned` trait, maybe I'll try to figure that out.

---

_@zanieb reviewed on 2024-03-26 01:42_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:95 on 2024-03-26 01:42_

I think it needs to be generic because of the crate dependencies, I didn't do it to have an empty one. If it's not generic, then `uv-resolver` has a dependency on `uv-installer` which afaik is a no-no and the purpose of the `uv-traits` crate.



---

_@zanieb reviewed on 2024-03-26 01:42_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolution.rs`:151 on 2024-03-26 01:42_

Yeah definitely not, thanks!

---

_@zanieb reviewed on 2024-03-26 01:45_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 01:45_

e.g.

```diff
diff --git a/crates/distribution-types/src/resolved.rs b/crates/distribution-types/src/resolved.rs
index fda37bc4..bf7d897d 100644
--- a/crates/distribution-types/src/resolved.rs
+++ b/crates/distribution-types/src/resolved.rs
@@ -33,8 +33,10 @@ impl ResolvedDist {
     }
 }
 
-impl ResolvedDistRef<'_> {
-    pub fn as_owned(&self) -> ResolvedDist {
+impl ToOwned for ResolvedDistRef<'_> {
+    type Owned = ResolvedDist;
+
+    fn to_owned(&self) -> Self::Owned {
         match self {
             Self::Installable(dist) => ResolvedDist::Installable((*dist).clone()),
             Self::Installed(dist) => ResolvedDist::Installed((*dist).clone()),
```
```
    Checking distribution-types v0.0.1 (/Users/mz/workspace/uv/crates/distribution-types)
error[E0119]: conflicting implementations of trait `ToOwned` for type `resolved::ResolvedDistRef<'_>`
  --> crates/distribution-types/src/resolved.rs:36:1
   |
36 | impl ToOwned for ResolvedDistRef<'_> {
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: conflicting implementation in crate `alloc`:
           - impl<T> ToOwned for T
             where T: Clone;

For more information about this error, try `rustc --explain E0119`.
error: could not compile `distribution-types` (lib) due to 1 previous error
```

---

_@zanieb reviewed on 2024-03-26 01:52_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 01:52_

(just writing `to_owned` without implementing the trait is weird too...)

---

_@charliermarsh reviewed on 2024-03-26 02:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:95 on 2024-03-26 02:14_

Ahh okay, I see. That makes sense.

---

_@charliermarsh reviewed on 2024-03-26 02:14_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-26 02:14_

I honestly think `ResolvedDistRef` is ok.

---

_@charliermarsh reviewed on 2024-03-26 02:15_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 02:15_

Yeah `to_owned` may not be quite right... I've honestly never implemented the trait so I'm not sure. I do think it should be `to_` something though.

---

_@zanieb reviewed on 2024-03-26 03:46_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 03:46_

Yeah :/ I was pursuing it based on that concept you linked. Unfortunately there isn't a great `to_<...>` subject other than "owned". I'm leaning towards just `cloned()`.

Calling in @BurntSushi to tell me what to do.

---

_@zanieb reviewed on 2024-03-26 03:47_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:290 on 2024-03-26 03:47_

It started to feel a little silly once I saw how much we clone `Dist`, but I'm okay with having it for now.

---

_@zanieb reviewed on 2024-03-26 03:56_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 03:56_

Some interesting context in https://stackoverflow.com/questions/72105604/implement-toowned-for-user-defined-types

---

_@BurntSushi reviewed on 2024-03-26 14:49_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 14:49_

Yeah it's uncommon to impl `ToOwned` directly. I don't think there's any issue with a concrete `to_owned()` method though. `as_owned()` is definitely not in keeping with convention. 

You could also go the more concrete direction and name this `to_resolved_dist`. That's probably what I would do here I think. But `to_owned()` is also okay IMO.

`cloned()` doesn't feel quite right to me here, since this isn't just a clone, but a conversion.

---

_@zanieb reviewed on 2024-03-26 14:53_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 14:53_

When I did a concrete `to_owned` method it wasn't called — the trait one was called instead. Seems problematic to need to specify the type to lookup the method on?

---

_@BurntSushi reviewed on 2024-03-26 14:57_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 14:57_

Ahhhhh right okay. Yeah, I'd go with `to_resolved_dist` then personally.

---

_@zanieb reviewed on 2024-03-26 15:24_

---

_Review comment by @zanieb on `crates/distribution-types/src/resolved.rs`:42 on 2024-03-26 15:24_

I think this might be because the method wasn't public 😬 

---

_Marked ready for review by @zanieb on 2024-03-27 16:38_

---

_@zanieb reviewed on 2024-03-27 16:39_

---

_Review comment by @zanieb on `crates/distribution-types/src/installed.rs`:119 on 2024-03-27 16:39_

How likely is this to fail?

I'd rather not push more into this pull request but we panic on this later because we don't have the metadata error.

---

_@zanieb reviewed on 2024-03-27 16:42_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:192 on 2024-03-27 16:42_

I currently don't do any reporting on these yet, which matches our already-satisfied installation behavior in general.

I'd like to explore adding output in a follow-up.

---

_@zanieb reviewed on 2024-03-27 16:47_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1015 on 2024-03-27 16:47_

This could block this pull request. We could add a new error variant here until `dist.metadata()` is refactored.

---

_@zanieb reviewed on 2024-03-27 16:56_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:618 on 2024-03-27 16:56_

We don't short circuit anymore because it might be available in the installed packages.

---

_@zanieb reviewed on 2024-03-27 16:57_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:637 on 2024-03-27 16:57_

We need to initialize this for lifetimes since `version_map` is a reference.

---

_Review requested from @charliermarsh by @zanieb on 2024-03-27 17:05_

---

_@zanieb reviewed on 2024-03-27 21:08_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:637 on 2024-03-27 21:08_

Alternatively we could have `CandidateSelector.select` take an `Option<&VersionMap>` but I opted for an empty iterable to avoid nesting in `select` instead. I'd consider revisiting this in the future as well.

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:192 on 2024-03-27 21:09_

In theory, these should always be empty...

---

_Review comment by @charliermarsh on `crates/uv-installer/src/plan.rs`:404 on 2024-03-27 21:10_

Perhaps we should rename this to `installed`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:86 on 2024-03-27 21:10_

And prefer them over non-local packages, is my read. (But not over "preferences".)

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/exclusions.rs`:37 on 2024-03-27 21:12_

This might be slightly easier to write/read as a `match (upgrade, reinstall)`? But not strong feedback.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/version_map.rs`:202 on 2024-03-27 21:14_

I think this would more commonly be done by implementing the `Default` trait.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/venv.rs`:219 on 2024-03-27 21:15_

Hopefully never happens...

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-27 21:15_

Nice, and these just get left as-is.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-27 21:17_

I'm trying to think through how this intersects with `reinstalls` though... Like, what happens if we end up having to use a different version than one of the installed versions? How will the existing version be uninstalled? Would it already have been included in reinstalls somehow...?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-27 21:21_

Yeah, I think they would in theory already be included in `reinstalls`... Does that rely on the assumption that we pass exact-equality requirements into the `Planner`? We might already rely on this today in `pip install`?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:618 on 2024-03-27 21:22_

Does that come at a cost? I assume it's negligible?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1015 on 2024-03-27 21:23_

What's the blocker? E.g., why does it need to be refactored?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-27 21:26_

Like the problem I'm imagining is something like... We have `foo==2.0.0` installed. The user requests `foo`. `foo==2.0.0` satisfies `foo`, but then we go to resolve, and we find that we need `foo==3.0.0`. But we'd only learn that when resolving, not when creating the install plan...

It shouldn't matter here since it's not a "real" resolution, we're just looking for a distribution for each version. It could matter for `pip install`, but in that case we resolve first, then use the `requirements` from the resolution as the inputs to the plan, which are pinned to exact versions. Anyway, there's probably some (existing) implicit coupling there that we should consider making explicit.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:637 on 2024-03-27 21:27_

I _think_ you can do `let empty_version_map;` here and then `empty_version_map = VersionMap::default()` in each branch if you want.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:862 on 2024-03-27 21:31_

I believe so but want to confirm: do you have any tests in which a package is already installed, but doesn't exist on the registry? I.e., exercising true for `self.unavailable_packages.get(package_name).is_some()` and false for the latter condition.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:866 on 2024-03-27 21:31_

How do we even get here btw?

---

_@charliermarsh approved on 2024-03-27 21:32_

This generally looks great to me.

---

_@zanieb reviewed on 2024-03-27 23:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:618 on 2024-03-27 23:48_

I think it's a small cost, but I think this wasn't an optimization really it was just a "we don't have a version map so we can't call select". We still short circuit if no versions are found locally.

---

_@zanieb reviewed on 2024-03-27 23:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:637 on 2024-03-27 23:48_

Oh that could be nicer.

---

_@zanieb reviewed on 2024-03-27 23:50_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:192 on 2024-03-27 23:50_

Do you mean just in dispatch or..?

I populate `local` at https://github.com/astral-sh/uv/blob/5a363fc2d4bccd2e049acc1fcfa18ed24fbd6e4a/crates/uv-installer/src/plan.rs#L177

---

_@zanieb reviewed on 2024-03-27 23:53_

---

_Review comment by @zanieb on `crates/uv-installer/src/plan.rs`:404 on 2024-03-27 23:53_

I think I had that initially and changed to `local` to opposed `remote`. I don't really like `remote` either since they could be on the user's machine just not in the environment... hm.

---

_@zanieb reviewed on 2024-03-27 23:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-27 23:57_

Since #2696 was merged I'm just telling the resolver that there are no installed packages available for `pip sync`. We can consider how we want to handle that separately?

Do your comments apply outside of this command?

---

_@charliermarsh reviewed on 2024-03-27 23:58_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:192 on 2024-03-27 23:58_

I just mean in `dispatch`, since it's a fresh virtual environment IIUC.

---

_@charliermarsh reviewed on 2024-03-28 00:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:866 on 2024-03-28 00:00_

Maybe a dumb question since there's a `debug_assert!` here anyway.

---

_@charliermarsh reviewed on 2024-03-28 00:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-28 00:01_

I don't think my comments merit any changes to the PR. I'm more commenting on the fact that we pass `requirements` to `Plan` to tell it what versions we're going to need, but a `Requirement` is actually pretty loose.

---

_@zanieb reviewed on 2024-03-28 00:10_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:1015 on 2024-03-28 00:10_

Unblocked in https://github.com/astral-sh/uv/pull/2596/commits/a8ba64f0d4a650ec7c9cc6bc1331f014103da3a4 which should clarify

---

_@zanieb reviewed on 2024-03-28 00:12_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:637 on 2024-03-28 00:12_

Actually idk if that makes sense we're trying to return `&VersionMap::default()` in the match which is the problem?

---

_@zanieb reviewed on 2024-03-28 00:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:866 on 2024-03-28 00:48_

Ah if a version map cannot be retrieved for a package we track the package in `unavailable_packages` but that's no longer the sole way to have a candidate for a package so this is kind of a lazy way of allowing installed packages dependencies to be retrieved even if they were not available in the index.

---

_@zanieb reviewed on 2024-03-28 00:49_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:862 on 2024-03-28 00:49_

Yes definitely.

---

_@zanieb reviewed on 2024-03-28 00:50_

---

_Review comment by @zanieb on `crates/uv-resolver/src/candidate_selector.rs`:86 on 2024-03-28 00:50_

https://github.com/astral-sh/uv/pull/2596/commits/76fe2f43d5380e84a119af45df4dc6ba706cc500

---

_@zanieb reviewed on 2024-03-28 03:21_

---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:192 on 2024-03-28 03:21_

Ah okay yeah that makes sense. I commented on an arbitrary line here.

---

_@zanieb reviewed on 2024-03-28 14:29_

---

_Review comment by @zanieb on `crates/uv-installer/src/plan.rs`:404 on 2024-03-28 14:29_

This was for a bad reason, it collided with an `installed` variable in `Planner::build`. Resolved now.

---

_@zanieb reviewed on 2024-03-28 14:31_

---

_Review comment by @zanieb on `crates/uv-resolver/src/exclusions.rs`:37 on 2024-03-28 14:31_

Going to stick with the existing style since it is a copy of what we were doing before this pull request. I wouldn't be upset about a follow-up.

---

_@zanieb reviewed on 2024-03-28 14:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_sync.rs`:319 on 2024-03-28 14:32_

Yeah I noticed that too, like I kind of wanted the `ResolvedDist` here since the resolver _knows_ an installed version should be used already. I think there's probably room for future clean-up here.

---

_Comment by @zanieb on 2024-03-28 17:10_

I've addressed all the feedback from the review.

---

_Label `enhancement` added by @zanieb on 2024-03-28 17:10_

---

_Merged by @zanieb on 2024-03-28 18:49_

---

_Closed by @zanieb on 2024-03-28 18:49_

---

_Branch deleted on 2024-03-28 18:49_

---
