```yaml
number: 2066
title: Track source incompatibilities in prioritized distribution
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
draft: true
base: main
head: charlie/source
created_at: 2024-02-29T00:32:28Z
updated_at: 2024-03-08T20:54:44Z
url: https://github.com/astral-sh/uv/pull/2066
synced_at: 2026-01-12T16:04:51Z
```

# Track source incompatibilities in prioritized distribution

---

_@charliermarsh_

## Summary

Like with wheels, we can track the compatibility of source distributions in `PrioritizedDist` and use those to both backtrack _and_ surface better error messages to users.

Closes https://github.com/astral-sh/uv/issues/2003.


---

_Review requested from @zanieb by @charliermarsh on 2024-02-29 00:39_

---

_Label `bug` added by @charliermarsh on 2024-02-29 00:39_

---

_Marked ready for review by @charliermarsh on 2024-02-29 00:39_

---

_@charliermarsh reviewed on 2024-02-29 00:43_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:47 on 2024-02-29 00:43_

Interestingly, if we track `RequiresPython` here, error messages regress quite a bit, because we no longer track the Python incompatibility in the resolver:

```diff
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: compile_python_37
Source: crates/uv/tests/pip_compile.rs:656
────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    2     2 │ ----- stdout -----
    3     3 │
    4     4 │ ----- stderr -----
    5     5 │   × No solution found when resolving dependencies:
    6       │-  ╰─▶ Because the requested Python version (3.7) does not satisfy Python>=3.8
    7       │-      and black==23.10.1 depends on Python>=3.8, we can conclude that
    8       │-      black==23.10.1 cannot be used.
    9       │-      And because you require black==23.10.1, we can conclude that the
   10       │-      requirements are unsatisfiable.
          6 │+  ╰─▶ Because black==23.10.1 is unusable because no source distributions
          7 │+      are available that meet your required Python version and you require
          8 │+      black==23.10.1, we can conclude that the requirements are unsatisfiable.
```

Here's the diff, for posterity:
```diff
diff --git a/crates/distribution-types/src/prioritized_distribution.rs b/crates/distribution-types/src/prioritized_distribution.rs
index 148e82cd..13a516a8 100644
--- a/crates/distribution-types/src/prioritized_distribution.rs
+++ b/crates/distribution-types/src/prioritized_distribution.rs
@@ -45,6 +45,7 @@ pub enum SourceCompatibility {
 #[derive(Debug, PartialEq, Eq, Ord, PartialOrd, Clone, Copy)]
 pub enum IncompatibleSource {
     NoBuild,
+    RequiresPython,
 }
 
 #[derive(Debug, PartialEq, Eq, Clone, Copy)]
diff --git a/crates/uv-resolver/src/resolver/mod.rs b/crates/uv-resolver/src/resolver/mod.rs
index 7bc5c710..e7e02fc1 100644
--- a/crates/uv-resolver/src/resolver/mod.rs
+++ b/crates/uv-resolver/src/resolver/mod.rs
@@ -18,10 +18,7 @@ use tracing::{debug, info_span, instrument, trace, warn, Instrument};
 use url::Url;
 
 use distribution_filename::WheelFilename;
-use distribution_types::{
-    BuiltDist, Dist, DistributionMetadata, Incompatible, IncompatibleSource, IncompatibleWheel,
-    Name, RemoteSource, SourceDist, VersionOrUrl,
-};
+use distribution_types::{BuiltDist, Dist, DistributionMetadata, Incompatible, IncompatibleSource, IncompatibleWheel, Name, RemoteSource, SourceDist, VersionOrUrl};
 use pep440_rs::{Version, VersionSpecifiers, MIN_VERSION};
 use pep508_rs::{MarkerEnvironment, Requirement};
 use platform_tags::{IncompatibleTag, Tags};
@@ -393,6 +390,7 @@ impl<'a, Provider: ResolverProvider> Resolver<'a, Provider> {
                                             IncompatibleTag::Platform => "no wheels are available with a matching platform".to_string(),
                                         }
                                     }
+                                    Incompatible::Source(IncompatibleSource::RequiresPython) => "no source distributions are available that meet your required Python version".to_string(),
                                     Incompatible::Source(IncompatibleSource::NoBuild) => "building from source is disabled".to_string(),
                                 }
                             } else {
@@ -663,7 +661,7 @@ impl<'a, Provider: ResolverProvider> Resolver<'a, Provider> {
                         // If the version is incompatible because no distributions match, exit early.
                         return Ok(Some(ResolverVersion::Unavailable(
                             candidate.version().clone(),
-                            UnavailableVersion::NoDistributions(*incompatibility),
+                            UnavailableVersion::NoDistributions(incompatibility.clone()),
                         )));
                     }
                 };
diff --git a/crates/uv-resolver/src/version_map.rs b/crates/uv-resolver/src/version_map.rs
index e8a1cc02..b742daaf 100644
--- a/crates/uv-resolver/src/version_map.rs
+++ b/crates/uv-resolver/src/version_map.rs
@@ -391,6 +391,8 @@ impl VersionMapLazy {
                     DistFilename::SourceDistFilename(filename) => {
                         let compatibility = if self.no_build {
                             SourceCompatibility::Incompatible(IncompatibleSource::NoBuild)
+                        } else if file.requires_python.as_ref().is_some_and(|requires_python| !requires_python.contains(self.python_requirement.target())) {
+                            SourceCompatibility::Incompatible(IncompatibleSource::RequiresPython)
                         } else {
                             SourceCompatibility::Compatible
                         };
@@ -400,13 +402,7 @@ impl VersionMapLazy {
                             file,
                             self.index.clone(),
                         );
-                        priority_dist.insert_source(
-                            dist,
-                            requires_python,
-                            yanked,
-                            Some(hash),
-                            compatibility,
-                        );
+                        priority_dist.insert_source(dist, requires_python, yanked, Some(hash), compatibility);
                     }
                 }
             }
```

---

_@charliermarsh reviewed on 2024-02-29 00:44_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:47 on 2024-02-29 00:44_

This actually suggests to me that we shouldn't be tracking `RequiresPython` for wheels like we are in this file, but it's a little complicated... I've played with a few refactors.

---

_Comment by @charliermarsh on 2024-02-29 02:26_

I think this should actually be closed in favor of https://github.com/astral-sh/uv/pull/1298 which solves _at least_ this problem and a few others. Gonna leave it in draft for now though until Zanie is back.

---

_Converted to draft by @charliermarsh on 2024-02-29 02:26_

---

_Comment by @charliermarsh on 2024-03-08 20:54_

Closed in favor of https://github.com/astral-sh/uv/pull/1298.

---

_Closed by @charliermarsh on 2024-03-08 20:54_

---

_Branch deleted on 2024-03-08 20:54_

---
