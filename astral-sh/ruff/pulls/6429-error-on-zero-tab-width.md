```yaml
number: 6429
title: Error on zero tab width
type: pull_request
state: merged
author: tjkuson
labels:
  - bug
assignees: []
merged: true
base: main
head: zero-tab-width
created_at: 2023-08-08T16:07:35Z
updated_at: 2023-08-15T09:10:42Z
url: https://github.com/astral-sh/ruff/pull/6429
synced_at: 2026-01-12T02:52:04Z
```

# Error on zero tab width

---

_Pull request opened by @tjkuson on 2023-08-08 16:07_

## Summary

Error if `tab-size` is set to zero (it is used as a divisor). Closes #6423.

Also fixes a typo.

## Test Plan

Running ruff with a config

```toml
[tool.ruff]
tab-size = 0
```

returns an error message to the user saying that `tab-size` must be greater than zero.


---

_Comment by @github-actions[bot] on 2023-08-08 16:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.07ms     4.2 MB/sec    1.01      9.8±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1911.8±33.91µs     8.7 MB/sec    1.02   1942.9±4.19µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    218.1±8.19µs    13.5 MB/sec    1.26  274.0±191.19µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.11ms     6.2 MB/sec    1.00      4.1±0.09ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.1±0.06ms     3.4 MB/sec    1.02     12.3±0.09ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.2 MB/sec    1.01      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.7±1.70µs     6.6 MB/sec    1.01    454.1±4.27µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.04ms     4.6 MB/sec    1.01      5.6±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.03ms     6.5 MB/sec    1.01      6.3±0.02ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1334.2±4.50µs    12.5 MB/sec    1.02  1359.4±11.90µs    12.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.8±2.85µs    19.4 MB/sec    1.01    153.3±1.25µs    19.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.01ms     9.2 MB/sec    1.01      2.8±0.01ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.40ms     3.5 MB/sec    1.02     11.8±0.52ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.08ms     7.7 MB/sec    1.05      2.3±0.13ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   253.4±13.62µs    11.6 MB/sec    1.01   255.1±19.54µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.25ms     5.3 MB/sec    1.01      4.9±0.21ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.4±0.56ms     2.6 MB/sec    1.00     15.1±1.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.15ms     3.9 MB/sec    1.00      4.2±0.19ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   530.1±21.63µs     5.6 MB/sec    1.03   547.6±56.73µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.24ms     3.6 MB/sec    1.00      7.0±0.27ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.24ms     4.7 MB/sec    1.00      8.6±0.23ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1756.6±67.20µs     9.5 MB/sec    1.00  1736.4±59.37µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.9±9.35µs    14.0 MB/sec    1.05   220.5±11.37µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.10ms     6.9 MB/sec    1.02      3.8±0.15ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @tjkuson on 2023-08-08 17:39_

---

_Comment by @charliermarsh on 2023-08-08 18:45_

Awesome, thank you. I'm wondering, could we instead make it impossible to create a `TabSize` with a size of 0, by using `NonZeroU8` as the wrapped type? Something like this:

```diff
diff --git a/crates/ruff/src/line_width.rs b/crates/ruff/src/line_width.rs
index 8619b42aa..1700725ae 100644
--- a/crates/ruff/src/line_width.rs
+++ b/crates/ruff/src/line_width.rs
@@ -1,4 +1,6 @@
 use serde::{Deserialize, Serialize};
+use std::num::NonZeroU8;
+use std::ops::Deref;
 use unicode_width::UnicodeWidthChar;
 
 use ruff_macros::CacheKey;
@@ -83,7 +85,7 @@ impl LineWidth {
     }
 
     fn update(mut self, chars: impl Iterator<Item = char>) -> Self {
-        let tab_size: usize = self.tab_size.into();
+        let tab_size: usize = self.tab_size.as_usize();
         for c in chars {
             match c {
                 '\t' => {
@@ -144,22 +146,22 @@ impl PartialOrd<LineLength> for LineWidth {
 /// The size of a tab.
 #[derive(Clone, Copy, Debug, PartialEq, Eq, Serialize, Deserialize, CacheKey)]
 #[cfg_attr(feature = "schemars", derive(schemars::JsonSchema))]
-pub struct TabSize(pub u8);
+pub struct TabSize(pub NonZeroU8);
 
-impl Default for TabSize {
-    fn default() -> Self {
-        Self(4)
+impl TabSize {
+    fn as_usize(&self) -> usize {
+        self.0.get() as usize
     }
 }
 
-impl From<u8> for TabSize {
-    fn from(tab_size: u8) -> Self {
-        Self(tab_size)
+impl Default for TabSize {
+    fn default() -> Self {
+        Self(NonZeroU8::new(4).unwrap())
     }
 }
 
-impl From<TabSize> for usize {
-    fn from(tab_size: TabSize) -> Self {
-        tab_size.0 as usize
+impl From<NonZeroU8> for TabSize {
+    fn from(tab_size: NonZeroU8) -> Self {
+        Self(tab_size)
     }
 }
diff --git a/crates/ruff/src/message/text.rs b/crates/ruff/src/message/text.rs
index 3aa5ebc54..a8349cef4 100644
--- a/crates/ruff/src/message/text.rs
+++ b/crates/ruff/src/message/text.rs
@@ -1,6 +1,7 @@
 use std::borrow::Cow;
 use std::fmt::{Display, Formatter};
 use std::io::Write;
+use std::num::NonZeroU8;
 
 use annotate_snippets::display_list::{DisplayList, FormatOptions};
 use annotate_snippets::snippet::{Annotation, AnnotationType, Slice, Snippet, SourceAnnotation};
@@ -16,6 +17,7 @@ use crate::line_width::{LineWidth, TabSize};
 use crate::message::diff::Diff;
 use crate::message::{Emitter, EmitterContext, Message};
 use crate::registry::AsRule;
+use crate::rules::pycodestyle::rules::logical_lines::Whitespace::Tab;
 use crate::source_kind::SourceKind;
 
 bitflags! {
@@ -293,12 +295,10 @@ impl Display for MessageCodeFrame<'_> {
 }
 
 fn replace_whitespace(source: &str, annotation_range: TextRange) -> SourceCode {
-    static TAB_SIZE: TabSize = TabSize(4); // TODO(jonathan): use `tab-size`
-
     let mut result = String::new();
     let mut last_end = 0;
     let mut range = annotation_range;
-    let mut line_width = LineWidth::new(TAB_SIZE);
+    let mut line_width = LineWidth::new(TabSize::default());
 
     for (index, c) in source.char_indices() {
         let old_width = line_width.get();
diff --git a/crates/ruff_cache/src/cache_key.rs b/crates/ruff_cache/src/cache_key.rs
index e015112bd..c62e580d1 100644
--- a/crates/ruff_cache/src/cache_key.rs
+++ b/crates/ruff_cache/src/cache_key.rs
@@ -2,6 +2,7 @@ use std::borrow::Cow;
 use std::collections::hash_map::DefaultHasher;
 use std::collections::{BTreeMap, BTreeSet, HashMap, HashSet};
 use std::hash::{Hash, Hasher};
+use std::num::NonZeroU8;
 use std::ops::{Deref, DerefMut};
 use std::path::{Path, PathBuf};
 
@@ -205,6 +206,13 @@ impl CacheKey for i8 {
     }
 }
 
+impl CacheKey for NonZeroU8 {
+    #[inline]
+    fn cache_key(&self, state: &mut CacheKeyHasher) {
+        state.write_u8(self.get());
+    }
+}
+
 macro_rules! impl_cache_key_tuple {
     () => (
         impl CacheKey for () {
```

---

_Merged by @charliermarsh on 2023-08-08 20:51_

---

_Closed by @charliermarsh on 2023-08-08 20:51_

---

_Comment by @charliermarsh on 2023-08-08 20:51_

Awesome, thanks!

---

_Label `bug` added by @charliermarsh on 2023-08-08 20:51_

---

_Branch deleted on 2023-08-15 09:10_

---
