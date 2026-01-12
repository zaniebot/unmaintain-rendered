```yaml
number: 97
title: Migrate resolver proof-of-concept to PubGrub
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/pubgrub
created_at: 2023-10-16T01:49:01Z
updated_at: 2023-10-16T02:14:55Z
url: https://github.com/astral-sh/uv/pull/97
synced_at: 2026-01-12T16:03:45Z
```

# Migrate resolver proof-of-concept to PubGrub

---

_@charliermarsh_

## Summary

This PR enables the proof-of-concept resolver to backtrack by way of using the `pubgrub-rs` crate.

Rather than using PubGrub as a _framework_ (implementing the `DependencyProvider` trait, letting PubGrub call us), I've instead copied over PubGrub's primary solver hook (which is only ~100 lines or so) and modified it for our purposes (e.g., made it async).

There's a lot to improve here, but it's a start that will let us understand PubGrub's appropriateness for this problem space. A few observations:

- In simple cases, the resolver is slower than our current (naive) resolver. I think it's just that the pipelining isn't as efficient as in the naive case, where we can just stream package and version fetches concurrently without any bottlenecks.
- A lot of the code here relates to bridging PubGrub with our own abstractions -- so we need a `PubGrubPackage`, a `PubGrubVersion`, etc.

## Appendix

I've vendored the crate to enable us to make some small changes to it without maintaining a fork (for now). Specifically, I copied over `pubgrub-rs` at `717289be5722dd5caaa0d1f4ed13047d11a7f7fd`, and then made these changes, which only modify visibility and make some traits async-friendly:

```diff
diff --git a/vendor/pubgrub/src/error.rs b/vendor/pubgrub/src/error.rs
index 0553d8d..d75f1b9 100644
--- a/vendor/pubgrub/src/error.rs
+++ b/vendor/pubgrub/src/error.rs
@@ -27,7 +27,7 @@ pub enum PubGrubError<P: Package, V: Version> {
         version: V,
         /// Error raised by the implementer of
         /// [DependencyProvider](crate::solver::DependencyProvider).
-        source: Box<dyn std::error::Error>,
+        source: Box<dyn std::error::Error + Send + Sync>,
     },
 
     /// Error arising when the implementer of
@@ -63,12 +63,12 @@ pub enum PubGrubError<P: Package, V: Version> {
     /// returned an error in the method
     /// [choose_package_version](crate::solver::DependencyProvider::choose_package_version).
     #[error("Decision making failed")]
-    ErrorChoosingPackageVersion(Box<dyn std::error::Error>),
+    ErrorChoosingPackageVersion(Box<dyn std::error::Error + Send + Sync>),
 
     /// Error arising when the implementer of [DependencyProvider](crate::solver::DependencyProvider)
     /// returned an error in the method [should_cancel](crate::solver::DependencyProvider::should_cancel).
     #[error("We should cancel")]
-    ErrorInShouldCancel(Box<dyn std::error::Error>),
+    ErrorInShouldCancel(Box<dyn std::error::Error + Send + Sync>),
 
     /// Something unexpected happened.
     #[error("{0}")]
diff --git a/vendor/pubgrub/src/internal/incompatibility.rs b/vendor/pubgrub/src/internal/incompatibility.rs
index acf900b..f0f7d62 100644
--- a/vendor/pubgrub/src/internal/incompatibility.rs
+++ b/vendor/pubgrub/src/internal/incompatibility.rs
@@ -36,7 +36,7 @@ pub struct Incompatibility<P: Package, V: Version> {
 }
 
 /// Type alias of unique identifiers for incompatibilities.
-pub type IncompId<P, V> = Id<Incompatibility<P, V>>;
+pub(crate) type IncompId<P, V> = Id<Incompatibility<P, V>>;
 
 #[derive(Debug, Clone)]
 enum Kind<P: Package, V: Version> {
diff --git a/vendor/pubgrub/src/internal/mod.rs b/vendor/pubgrub/src/internal/mod.rs
index 86d7e22..4c4fc55 100644
--- a/vendor/pubgrub/src/internal/mod.rs
+++ b/vendor/pubgrub/src/internal/mod.rs
@@ -2,9 +2,9 @@
 
 //! Non exposed modules.
 
-pub mod arena;
-pub mod core;
-pub mod incompatibility;
-pub mod partial_solution;
-pub mod small_map;
-pub mod small_vec;
+pub(crate) mod arena;
+pub(crate) mod core;
+pub(crate) mod incompatibility;
+pub(crate) mod partial_solution;
+pub(crate) mod small_map;
+pub(crate) mod small_vec;
diff --git a/vendor/pubgrub/src/internal/partial_solution.rs b/vendor/pubgrub/src/internal/partial_solution.rs
index ad45424..390fcac 100644
--- a/vendor/pubgrub/src/internal/partial_solution.rs
+++ b/vendor/pubgrub/src/internal/partial_solution.rs
@@ -44,7 +44,7 @@ struct PackageAssignments<P: Package, V: Version> {
 }
 
 #[derive(Clone, Debug)]
-pub struct DatedDerivation<P: Package, V: Version> {
+pub(crate) struct DatedDerivation<P: Package, V: Version> {
     global_index: u32,
     decision_level: DecisionLevel,
     cause: IncompId<P, V>,
@@ -93,7 +93,7 @@ impl<P: Package, V: Version> PartialSolution<P, V> {
             }
         }
         self.current_decision_level = self.current_decision_level.increment();
-        let mut pa = self
+        let pa = self
             .package_assignments
             .get_mut(&package)
             .expect("Derivations must already exist");
@@ -123,7 +123,7 @@ impl<P: Package, V: Version> PartialSolution<P, V> {
         self.next_global_index += 1;
         match self.package_assignments.entry(package) {
             Entry::Occupied(mut occupied) => {
-                let mut pa = occupied.get_mut();
+                let pa = occupied.get_mut();
                 pa.highest_decision_level = self.current_decision_level;
                 match &mut pa.assignments_intersection {
                     // Check that add_derivation is never called in the wrong context.
diff --git a/vendor/pubgrub/src/internal/small_map.rs b/vendor/pubgrub/src/internal/small_map.rs
index a1fe5f9..702e7e2 100644
--- a/vendor/pubgrub/src/internal/small_map.rs
+++ b/vendor/pubgrub/src/internal/small_map.rs
@@ -2,7 +2,7 @@ use crate::type_aliases::Map;
 use std::hash::Hash;
 
 #[derive(Debug, Clone)]
-pub enum SmallMap<K, V> {
+pub(crate) enum SmallMap<K, V> {
     Empty,
     One([(K, V); 1]),
     Two([(K, V); 2]),
@@ -10,7 +10,7 @@ pub enum SmallMap<K, V> {
 }
 
 impl<K: PartialEq + Eq + Hash, V> SmallMap<K, V> {
-    pub fn get(&self, key: &K) -> Option<&V> {
+    pub(crate) fn get(&self, key: &K) -> Option<&V> {
         match self {
             Self::Empty => None,
             Self::One([(k, v)]) if k == key => Some(v),
@@ -22,7 +22,7 @@ impl<K: PartialEq + Eq + Hash, V> SmallMap<K, V> {
         }
     }
 
-    pub fn get_mut(&mut self, key: &K) -> Option<&mut V> {
+    pub(crate) fn get_mut(&mut self, key: &K) -> Option<&mut V> {
         match self {
             Self::Empty => None,
             Self::One([(k, v)]) if k == key => Some(v),
@@ -34,7 +34,7 @@ impl<K: PartialEq + Eq + Hash, V> SmallMap<K, V> {
         }
     }
 
-    pub fn remove(&mut self, key: &K) -> Option<V> {
+    pub(crate) fn remove(&mut self, key: &K) -> Option<V> {
         let out;
         *self = match std::mem::take(self) {
             Self::Empty => {
@@ -70,7 +70,7 @@ impl<K: PartialEq + Eq + Hash, V> SmallMap<K, V> {
         out
     }
 
-    pub fn insert(&mut self, key: K, value: V) {
+    pub(crate) fn insert(&mut self, key: K, value: V) {
         *self = match std::mem::take(self) {
             Self::Empty => Self::One([(key, value)]),
             Self::One([(k, v)]) => {
@@ -108,7 +108,7 @@ impl<K: Clone + PartialEq + Eq + Hash, V: Clone> SmallMap<K, V> {
     /// apply the provided function to both values.
     /// If the result is None, remove that key from the merged map,
     /// otherwise add the content of the Some(_).
-    pub fn merge<'a>(
+    pub(crate) fn merge<'a>(
         &'a mut self,
         map_2: impl Iterator<Item = (&'a K, &'a V)>,
         f: impl Fn(&V, &V) -> Option<V>,
@@ -136,7 +136,7 @@ impl<K, V> Default for SmallMap<K, V> {
 }
 
 impl<K, V> SmallMap<K, V> {
-    pub fn len(&self) -> usize {
+    pub(crate) fn len(&self) -> usize {
         match self {
             Self::Empty => 0,
             Self::One(_) => 1,
@@ -147,7 +147,7 @@ impl<K, V> SmallMap<K, V> {
 }
 
 impl<K: Eq + Hash + Clone, V: Clone> SmallMap<K, V> {
-    pub fn as_map(&self) -> Map<K, V> {
+    pub(crate) fn as_map(&self) -> Map<K, V> {
         match self {
             Self::Empty => Map::default(),
             Self::One([(k, v)]) => {
@@ -184,7 +184,7 @@ impl<'a, K: 'a, V: 'a> Iterator for IterSmallMap<'a, K, V> {
 }
 
 impl<K, V> SmallMap<K, V> {
-    pub fn iter(&self) -> impl Iterator<Item = (&K, &V)> {
+    pub(crate) fn iter(&self) -> impl Iterator<Item = (&K, &V)> {
         match self {
             Self::Empty => IterSmallMap::Inline([].iter()),
             Self::One(data) => IterSmallMap::Inline(data.iter()),
diff --git a/vendor/pubgrub/src/solver.rs b/vendor/pubgrub/src/solver.rs
index 6201591..7f110b7 100644
--- a/vendor/pubgrub/src/solver.rs
+++ b/vendor/pubgrub/src/solver.rs
@@ -70,8 +70,8 @@ use std::collections::{BTreeMap, BTreeSet as Set};
 use std::error::Error;
 
 use crate::error::PubGrubError;
-use crate::internal::core::State;
-use crate::internal::incompatibility::Incompatibility;
+pub use crate::internal::core::State;
+pub use crate::internal::incompatibility::Incompatibility;
 use crate::package::Package;
 use crate::range::Range;
 use crate::type_aliases::{Map, SelectedDependencies};
@@ -249,7 +249,7 @@ pub trait DependencyProvider<P: Package, V: Version> {
     fn choose_package_version<T: Borrow<P>, U: Borrow<Range<V>>>(
         &self,
         potential_packages: impl Iterator<Item = (T, U)>,
-    ) -> Result<(T, Option<V>), Box<dyn Error>>;
+    ) -> Result<(T, Option<V>), Box<dyn Error + Send + Sync>>;
 
     /// Retrieves the package dependencies.
     /// Return [Dependencies::Unknown] if its dependencies are unknown.
@@ -257,14 +257,14 @@ pub trait DependencyProvider<P: Package, V: Version> {
         &self,
         package: &P,
         version: &V,
-    ) -> Result<Dependencies<P, V>, Box<dyn Error>>;
+    ) -> Result<Dependencies<P, V>, Box<dyn Error + Send + Sync>>;
 
     /// This is called fairly regularly during the resolution,
     /// if it returns an Err then resolution will be terminated.
     /// This is helpful if you want to add some form of early termination like a timeout,
     /// or you want to add some form of user feedback if things are taking a while.
     /// If not provided the resolver will run as long as needed.
-    fn should_cancel(&self) -> Result<(), Box<dyn Error>> {
+    fn should_cancel(&self) -> Result<(), Box<dyn Error + Send + Sync>> {
         Ok(())
     }
 }
@@ -367,7 +367,7 @@ impl<P: Package, V: Version> DependencyProvider<P, V> for OfflineDependencyProvi
     fn choose_package_version<T: Borrow<P>, U: Borrow<Range<V>>>(
         &self,
         potential_packages: impl Iterator<Item = (T, U)>,
-    ) -> Result<(T, Option<V>), Box<dyn Error>> {
+    ) -> Result<(T, Option<V>), Box<dyn Error + Send + Sync>> {
         Ok(choose_package_with_fewest_versions(
             |p| {
                 self.dependencies
@@ -385,7 +385,7 @@ impl<P: Package, V: Version> DependencyProvider<P, V> for OfflineDependencyProvi
         &self,
         package: &P,
         version: &V,
-    ) -> Result<Dependencies<P, V>, Box<dyn Error>> {
+    ) -> Result<Dependencies<P, V>, Box<dyn Error + Send + Sync>> {
         Ok(match self.dependencies(package, version) {
             None => Dependencies::Unknown,
             Some(dependencies) => Dependencies::Known(dependencies),
```

---

_@charliermarsh reviewed on 2023-10-16 01:49_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/wheel_finder.rs`:30 on 2023-10-16 01:49_

This is basically the previous resolver with the `--no-deps` flag set. It's used in `puffin sync`, where you have a resolved set of packages, and need to find the wheels to install. I think it's nice to keep this separate from the core resolver.

---

_Merged by @charliermarsh on 2023-10-16 02:05_

---

_Closed by @charliermarsh on 2023-10-16 02:05_

---

_Branch deleted on 2023-10-16 02:05_

---

_Comment by @MichaReiser on 2023-10-16 02:07_

I haven't looked at the code and only read the summary: 

What's the benefit of making the resolver async? I would expect that resolving is mainly CPU bound. Or is it because we intend of making network requests as part of the resolver phase?

---

_Comment by @charliermarsh on 2023-10-16 02:12_

We need to make (a substantial number of) network requests as part of the resolver phase. We don't have access to the entire package index upfront.


---

_Comment by @charliermarsh on 2023-10-16 02:13_

Resolving is mainly I/O bound. We are also considering writing our own endpoints to serve the metadata more efficiently (e.g., serve all metadata for all versions of a given package in a single request, rather than requiring a separate request for each version).

---

_Comment by @charliermarsh on 2023-10-16 02:14_

Sorry, "Resolving is mainly I/O bound" is not really true. It's I/O bound in simple cases. I assume it is CPU bound in more complex cases.

---
