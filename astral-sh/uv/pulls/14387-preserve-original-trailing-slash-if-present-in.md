```yaml
number: 14387
title: "Preserve original trailing slash if present in `find-links` URLs"
type: pull_request
state: closed
author: jtfmumm
labels:
  - bug
assignees: []
base: main
head: jtfm/add-trailing-slash-for-find-links
created_at: 2025-07-01T09:25:05Z
updated_at: 2025-07-09T09:49:07Z
url: https://github.com/astral-sh/uv/pull/14387
synced_at: 2026-01-12T16:11:12Z
```

# Preserve original trailing slash if present in `find-links` URLs

---

_@jtfmumm_

As part of #14245 and #14346, uv was normalizing trailing slashes for find-links URLs by removing them. However, find-links URLs have different meanings depending on whether a trailing slash is present (as evidenced in 301 redirects from PyPI and #14367). This PR preserves trailing slashes if present in find-links URLs.
 
Fixes #14367


---

_Assigned to @jtfmumm by @jtfmumm on 2025-07-01 09:25_

---

_Unassigned @jtfmumm by @jtfmumm on 2025-07-01 09:25_

---

_Label `bug` added by @jtfmumm on 2025-07-01 09:25_

---

_Label `do-not-merge` added by @jtfmumm on 2025-07-01 09:27_

---

_Comment by @charliermarsh on 2025-07-03 13:13_

I think relying on `url.given()` is dangerous, I don't think that persists through serialization / deserialization. I think we need to avoid stripping the trailing slash for `--find-links` URLs in the first place.

---

_Renamed from "Add original trailing slash if missing before `find-links` request" to "Preserve original trailing slash if present on `find-links` URLs" by @jtfmumm on 2025-07-04 08:06_

---

_@jtfmumm reviewed on 2025-07-04 08:54_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:130 on 2025-07-04 08:54_

This logic was moved from the `From<VerbatimUrl>` implementation. It's unchanged except that the trailing slash is only removed if `TrailingSlashPolicy::Remove` is passed in.

---

_Comment by @jtfmumm on 2025-07-04 08:56_

> I think relying on `url.given()` is dangerous, I don't think that persists through serialization / deserialization. I think we need to avoid stripping the trailing slash for `--find-links` URLs in the first place.

Updated. I've also updated the `VerbatimUrl` rustdocs in #14456 to make this behavior clearer.

---

_Renamed from "Preserve original trailing slash if present on `find-links` URLs" to "Preserve original trailing slash if present in `find-links` URLs" by @jtfmumm on 2025-07-04 09:03_

---

_Label `do-not-merge` removed by @jtfmumm on 2025-07-04 09:06_

---

_Marked ready for review by @jtfmumm on 2025-07-04 09:06_

---

_@jtfmumm reviewed on 2025-07-04 12:46_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-04 12:46_

I don't really like this name. Originally I had it as `parse_without_slash_normalization`. Could be `parse_preserve_trailing_slash` but I think `parse_preserve_slash` isn't enough information?

---

_@jtfmumm reviewed on 2025-07-04 12:47_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-04 12:47_

I avoided things like `parse_as_given` or `parse_exact` since there could have already been different kinds of normalization prior to this point? 

---

_@jtfmumm reviewed on 2025-07-04 12:47_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:124 on 2025-07-04 12:47_

Same point as above about the function name

---

_@charliermarsh reviewed on 2025-07-07 13:40_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:358 on 2025-07-07 13:40_

I think we should just get rid of this, since it seems like callers _must_ be cognizant of whether they want to preserve or remove the trailing slash.

---

_@charliermarsh reviewed on 2025-07-07 13:42_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-07 13:42_

What if we just use:

- `parse_simple_api`
- `parse_find_links`

And thus not require callers to pass in a trailing slash policy?

---

_@jtfmumm reviewed on 2025-07-07 14:12_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-07 14:12_

Yeah that's better. I can follow a similar pattern with the verbatim URL cases: e.g., `simple_api_from_verbatim_url`

---

_@charliermarsh reviewed on 2025-07-07 14:21_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-07 14:21_

Yeah or `from_simple_api_url`?

---

_@jtfmumm reviewed on 2025-07-07 15:54_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index_url.rs`:53 on 2025-07-07 15:54_

Updated

---

_@zanieb approved on 2025-07-07 18:47_

---

_@charliermarsh approved on 2025-07-08 01:22_

---

_Comment by @charliermarsh on 2025-07-08 01:25_

Actually, hmm, there may be one bug here. We call `without_trailing_slash` in `satisfies` on all index URLs. But I think that will now be wrong for `--find-links` URLs? So we'll incorrectly think that the lockfile needs to be updated, since there will be an index URL in the lockfile that doesn't match the "collection" of index URLs.

---

_Comment by @charliermarsh on 2025-07-08 01:27_

We should probably try to find a way to remove `without_trailing_slash` there.

---

_Comment by @jtfmumm on 2025-07-08 09:49_

> Actually, hmm, there may be one bug here. We call `without_trailing_slash` in `satisfies` on all index URLs. But I think that will now be wrong for `--find-links` URLs? So we'll incorrectly think that the lockfile needs to be updated, since there will be an index URL in the lockfile that doesn't match the "collection" of index URLs.

I think we still need a way to normalize the registry sources we read in from the lockfile, but at that point we no longer know whether it was a Simple API or find-links URL. This leads me to think we should only be normalizing at the point of lockfile validation. I've opened an [alternative PR](https://github.com/astral-sh/uv/pull/14503) that takes that approach instead.

---

_Comment by @charliermarsh on 2025-07-08 11:20_

I think normalizing the Simple API URLs is still a good thing, to be honest, since it minimizes churn. Should we just do _both_ things? Normalize, but also make the lockfile validation more lax? I had this in Discord:

```diff
diff --git a/crates/uv-distribution-types/src/file.rs b/crates/uv-distribution-types/src/file.rs
index 948378c0c..c227b4266 100644
--- a/crates/uv-distribution-types/src/file.rs
+++ b/crates/uv-distribution-types/src/file.rs
@@ -169,15 +169,6 @@ impl UrlString {
             .map(|(path, _)| Cow::Owned(UrlString(SmallString::from(path))))
             .unwrap_or(Cow::Borrowed(self))
     }
-
-    /// Return the [`UrlString`] (as a [`Cow`]) with trailing slash removed.
-    #[must_use]
-    pub fn without_trailing_slash(&self) -> Cow<'_, Self> {
-        self.as_ref()
-            .strip_suffix('/')
-            .map(|path| Cow::Owned(UrlString(SmallString::from(path))))
-            .unwrap_or(Cow::Borrowed(self))
-    }
 }
 
 impl AsRef<str> for UrlString {
@@ -270,18 +261,4 @@ mod tests {
             Cow::Owned(UrlString("https://example.com/path?query".into()))
         );
     }
-
-    #[test]
-    fn without_trailing_slash() {
-        // Borrows a URL without a slash
-        let url = UrlString("https://example.com/path".into());
-        assert_eq!(url.without_trailing_slash(), Cow::Borrowed(&url));
-
-        // Removes the trailing slash if present on the URL
-        let url = UrlString("https://example.com/path/".into());
-        assert_eq!(
-            url.without_trailing_slash(),
-            Cow::Owned(UrlString("https://example.com/path".into()))
-        );
-    }
 }
diff --git a/crates/uv-resolver/src/lock/mod.rs b/crates/uv-resolver/src/lock/mod.rs
index beeadc912..9ae3f0e99 100644
--- a/crates/uv-resolver/src/lock/mod.rs
+++ b/crates/uv-resolver/src/lock/mod.rs
@@ -1437,7 +1437,7 @@ impl Lock {
                     }
                     IndexUrl::Path(_) => None,
                 })
-                .collect::<BTreeSet<_>>()
+                .collect::<Vec<_>>()
         });
 
         let locals = indexes.map(|locations| {
@@ -1479,10 +1479,9 @@ impl Lock {
                 match index {
                     RegistrySource::Url(url) => {
                         // Normalize URL before validating.
-                        let url = url.without_trailing_slash();
                         if remotes
                             .as_ref()
-                            .is_some_and(|remotes| !remotes.contains(&url))
+                            .is_some_and(|remotes| !remotes.iter().any(|remote| matches!(remote.as_ref().strip_prefix(url.as_ref()), '' | '/')))
                         {
                             let name = &package.id.name;
                             let version = &package
```

---

_Comment by @zanieb on 2025-07-08 12:51_

If this change would invalidate the lockfile, we should add a test case covering that.

---

_Comment by @zanieb on 2025-07-08 12:52_

> at that point we no longer know whether it was a Simple API or find-links URL

This is separate, but... we should probably track this in the lockfile, right?

---

_Comment by @jtfmumm on 2025-07-08 12:58_

> I think normalizing the Simple API URLs is still a good thing, to be honest, since it minimizes churn. Should we just do both things? Normalize, but also make the lockfile validation more lax?

I'm a little concerned that we'll find the trailing slash difference could end up being significant for some implementation of the Simple API down the line, but maybe it's not likely. I've updated this PR to follow the more lax approach, though we need to check if `remote` is a prefix of `url` (for the case that the lockfile contains a slash for a Simple API URL).


---

_Comment by @jtfmumm on 2025-07-08 13:00_

> If this change would invalidate the lockfile, we should add a test case covering that.

Do you mean the more lax approach change? What scenario are you thinking of? 

There are tests that check different combinations of trailing slashes in user-provided URLs vs. lockfile URLs to ensure validation still succeeds. Those still pass with this change.

---

_Comment by @zanieb on 2025-07-08 13:22_

I'm referring to the scenario described in this comment: https://github.com/astral-sh/uv/pull/14387#issuecomment-3047046094

Is that not correct?

---

_Comment by @zanieb on 2025-07-08 13:23_

Regarding the "lax" approach, if a trailing slash on the find-links URL has semantic meaning, we should invalidate the lockfile if someone adds a trailing slash or removes it, right?

---

_Comment by @charliermarsh on 2025-07-08 13:25_

I considered that too, but I don't see how it would succeed in the first place if the trailing slash is required (or vice versa).

---

_Comment by @zanieb on 2025-07-08 13:34_

It could just change the result of the resolution, right? i.e., the find-links index returns no packages vs some packages?

---

_Comment by @charliermarsh on 2025-07-08 14:35_

I think that's possible in theory, it just seems really unlikely in practice. I agree that the right solution here is probably to track `--find-links` indexes with a different prefix in the lockfile. I don't know if we want to do that here, or in v0.8, or what. But we should _definitely_ get a fix out for this ASAP even if imperfect since it's breaking real user workflows.

---

_Comment by @jtfmumm on 2025-07-09 09:49_

Closing in favor of #14511

---

_Closed by @jtfmumm on 2025-07-09 09:49_

---
