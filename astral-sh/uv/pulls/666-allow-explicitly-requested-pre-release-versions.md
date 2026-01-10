```yaml
number: 666
title: Allow explicitly-requested pre-release versions
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/pre
created_at: 2023-12-15T20:06:24Z
updated_at: 2024-01-09T23:44:59Z
url: https://github.com/astral-sh/uv/pull/666
synced_at: 2026-01-10T15:44:44Z
```

# Allow explicitly-requested pre-release versions

---

_Pull request opened by @charliermarsh on 2023-12-15 20:06_

## Summary

This PR modifies our pre-release handling to enable resolving packages like `msgraph-sdk`.

Historically, by default, we've allowed pre-releases in two cases:

1. The package _only_ has pre-release versions. (This used to be the case for Black, most famously.)
2. The package is a first-party requirement, and includes a pre-release marker.

In either case, we chose the _most recent_ pre-release version.

This doesn't work for `msgraph-sdk`, because it depends on `msgraph-core>=1.0.0a2`. `msgraph-core` has a `0.2.2` stable release, so it doesn't qualify under Criteria 1; and because it's not a first-party dependency, it doesn't qualify under Criteria 2. The user could "fix" this by adding a direct dependency on `msgraph-core>=1.0.0a2`, and we could try to put up a better error message in this case... But I also think we can make pre-release handling smarter.

So, instead, this PR removes the if-necessary and explicit variants in favor of a single `IfRequested` variant, the semantics of which are as follows.

1. If a package _only_ has pre-release versions, accept the most recent compatible pre-release.
2. If _anyone_ asked for a pre-release version of the package (first-party, third-party, etc.), then allow pre-releases _if_ there are no stable releases in the range.

This leads to a much better resolution for `msgraph-sdk` without any user involvement (we select `msgraph-core==1.0.0a4`, the most recent pre-release; if a stable release is published, we'll then select that instead).

Determining whether _anyone_ asked for a pre-release version is tricky due to backtracking. Like, we can't just naively track which packages appear with a pre-release marker in _any_ package's requirements, because we might backtrack and remove the package with the pre-release dependency... The solution I opted for here is to iterate over the `Range`, look at the boundary, and check if _any_ of the boundary markers include pre-releases. We never make a non-pre-release marker into a pre-release marker in `PubGrubVersion::next`, so if any marker includes a pre-release, then _some_ requesting package included a pre-release. Presumedly, if we then backtrack, those ranges will be removed from the term, but this is honestly hard to test.

Closes https://github.com/astral-sh/puffin/issues/659.

---

_@charliermarsh reviewed on 2023-12-15 20:07_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:103 on 2023-12-15 20:07_

This requires a change in PubGrub, separate PR:

```diff
diff --git a/src/range.rs b/src/range.rs
index fc04e8b..b817f23 100644
--- a/src/range.rs
+++ b/src/range.rs
@@ -122,6 +122,23 @@ impl<V> Range<V> {
     pub fn is_empty(&self) -> bool {
         self.segments.is_empty()
     }
+
+    pub fn bounds(&self) -> impl Iterator<Item = &V> {
+        self.segments.iter().flat_map(|segment| {
+            let (v1, v2) = segment;
+            let v1 = match v1 {
+                Included(v) => Some(v),
+                Excluded(v) => Some(v),
+                Unbounded => None
+            };
+            let v2 = match v2 {
+                Included(v) => Some(v),
+                Excluded(v) => Some(v),
+                Unbounded => None
+            };
+            v1.into_iter().chain(v2)
+        })
+    }
 }
```

---

_Review requested from @zanieb by @charliermarsh on 2023-12-15 20:08_

---

_Review requested from @konstin by @charliermarsh on 2023-12-15 20:08_

---

_@charliermarsh reviewed on 2023-12-15 20:09_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:104 on 2023-12-15 20:09_

This will accept the most-compatible pre-release version, if there are no matching stable versions.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:106 on 2023-12-15 20:09_

This will accept the most-compatible pre-release version, if there are _no_ stable versions at all.

---

_@charliermarsh reviewed on 2023-12-15 20:09_

---

_@charliermarsh reviewed on 2023-12-15 20:15_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:103 on 2023-12-15 20:15_

I actually can't say _for sure_ that backtracking will avoid leaving pre-release markers in here. Like, if you have a dependency `foo`, and you try `foo==1.0.0`, and that adds a dependency on `bar>=1.0.0a1`... But you then backtrack, and remove `foo==1.0.0`, will `bar` _definitely_ not contain a pre-release marker anymore? 

---

_@charliermarsh reviewed on 2023-12-15 20:15_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:103 on 2023-12-15 20:15_

But that's the assumption that this rests upon.

---

_Comment by @charliermarsh on 2023-12-17 15:46_

@konstin and I discussed in Discord, and what's now proposed is:

- Prefer stable release over all pre-releases. If you're ever looking at a constraint, and there's a stable release that fits the constraints, ignore the pre-releases.
- If there are no stable releases in the range (regardless of whether the version marker included a pre-release explicitly), use the latest pre-release.

The primary difference between this and what's described in the PR summary is that we would no longer require that someone requested a pre-release explicitly (e.g., `foo>=1.0.0a1`). So it's a more permissive strategy, though one that _seems_ to adhere to PEP 508, which says: "accept remotely available pre-releases for version specifiers where there is no final or post release that satisfies the version specifier".

The main downside of this approach is that it... could be "wrong". Imagine we find ourselves in a situation where we have a pre-release that satisfies some constraints, and no stable releases. So we select the pre-release. It's possible that if we instead backtracked, we might find a global resolution that _doesn't_ require any pre-releases. I think handling that case would be _very_ hard to implement, maybe impossible? My hope is that this is minimally impactful in reality. But that's the reason we have the behavior we do on `main`: we need to know the set of packages that are _allowed_ to have pre-releases upfront.


---

_Comment by @charliermarsh on 2023-12-17 15:48_

I've pushed this updated strategy, though gonna hold on merging since I'd like @zanieb opinion.

---

_Comment by @charliermarsh on 2023-12-17 15:57_

I'm actually now worried that _either_ of these approaches will break PubGrub assumptions since we're now making it such that _more_ packages are available as the range gets constraint. And this suggests as much: https://pubgrub-rs-guide.netlify.app/limitations/prerelease_versions. We may need to stick to our current strategy and just try to improve the error message.

---

_Comment by @charliermarsh on 2023-12-17 16:11_

Okay, I'm now more convinced that we should close this and keep our current behavior. We'll need to add better error messages somehow.

---

_Comment by @charliermarsh on 2023-12-17 16:46_

We're gonna ask Jacob his perspective.

---

_Converted to draft by @charliermarsh on 2023-12-17 16:46_

---

_Comment by @zanieb on 2023-12-17 20:02_

> If there are no stable releases in the range (regardless of whether the version marker included a pre-release explicitly), use the latest pre-release.

This seems like bad behavior? I would be _very_ confused if a pre-release version got selected without an explicit request for a pre-release (whether that happens first or third party). Your original proposal makes much more sense to me.

Sounds like we'll be running into limitations of PubGrub here. I'm not opposed to moving forward with our current behavior and improved error messages i.e. requiring the user to explicitly opt into pre-release versions for the transitive dependency, but we may want to explore a better solution again in the future.

Perhaps interesting https://github.com/dart-lang/pub/pull/3078



---

_Comment by @charliermarsh on 2023-12-17 20:09_

> This seems like bad behavior? I would be very confused if a pre-release version got selected without an explicit request for a pre-release (whether that happens first or third party). Your original proposal makes much more sense to me.

For what it's worth, this is the behavior suggested in the standard (PEP 508).


---

_Closed by @charliermarsh on 2024-01-09 23:44_

---
