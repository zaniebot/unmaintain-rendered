```yaml
number: 15728
title: Improve the file watching failure error message
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/watch-err
created_at: 2025-01-24T18:04:56Z
updated_at: 2025-01-24T21:28:32Z
url: https://github.com/astral-sh/ruff/pull/15728
synced_at: 2026-01-12T15:55:52Z
```

# Improve the file watching failure error message

---

_@zanieb_

I really misunderstood this in https://github.com/astral-sh/ruff/pull/15664#issuecomment-2613079710

---

_Label `internal` added by @zanieb on 2025-01-24 18:04_

---

_Review requested from @carljm by @zanieb on 2025-01-24 18:04_

---

_Review requested from @MichaReiser by @zanieb on 2025-01-24 18:04_

---

_Review requested from @AlexWaygood by @zanieb on 2025-01-24 18:04_

---

_Review requested from @sharkdp by @zanieb on 2025-01-24 18:04_

---

_Comment by @zanieb on 2025-01-24 18:05_

I looked at rendering the expected event, but.. it's a bit harder.

---

_Comment by @AlexWaygood on 2025-01-24 18:28_

I'll leave this one to @MichaReiser 😆

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-01-24 18:28_

---

_@MichaReiser approved on 2025-01-24 18:30_

Nice, you could do

```patch
Index: crates/red_knot/tests/file_watching.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot/tests/file_watching.rs b/crates/red_knot/tests/file_watching.rs
--- a/crates/red_knot/tests/file_watching.rs	(revision 26f7e46dc8348ba17ba6db1a61e7c296e7906428)
+++ b/crates/red_knot/tests/file_watching.rs	(date 1737743417656)
@@ -1,5 +1,8 @@
 #![allow(clippy::disallowed_names)]
 
+use core::fmt;
+use std::fmt::Display;
 use std::io::Write;
 use std::time::{Duration, Instant};
 
@@ -39,15 +42,18 @@
     }
 
     #[track_caller]
-    fn stop_watch<M>(&mut self, matcher: M) -> Vec<ChangeEvent>
+    fn stop_watch<M>(&mut self, mut matcher: M) -> Vec<ChangeEvent>
     where
         M: MatchEvent,
     {
         // track_caller is unstable for lambdas -> That's why this is a fn
         #[track_caller]
-        fn panic_with_formatted_events(events: Vec<ChangeEvent>) -> Vec<ChangeEvent> {
+        fn panic_with_formatted_events(
+            expected: &str,
+            events: Vec<ChangeEvent>,
+        ) -> Vec<ChangeEvent> {
             panic!(
-                "Didn't observe expected change:\n{}",
+                "Didn't observe expected change: `{expected}`\n{}",
                 events
                     .into_iter()
                     .map(|event| format!("  - {event:?}"))
@@ -56,13 +62,15 @@
             )
         }
 
-        self.try_stop_watch(matcher, Duration::from_secs(10))
-            .unwrap_or_else(panic_with_formatted_events)
+        self.try_stop_watch(&mut matcher, Duration::from_secs(10))
+            .unwrap_or_else(|events| {
+                panic_with_formatted_events(&matcher.display().to_string(), events)
+            })
     }
 
     fn try_stop_watch<M>(
         &mut self,
-        mut matcher: M,
+        matcher: &mut M,
         timeout: Duration,
     ) -> Result<Vec<ChangeEvent>, Vec<ChangeEvent>>
     where
@@ -204,10 +212,26 @@
 
 trait MatchEvent {
     fn match_event(&mut self, event: &ChangeEvent) -> bool;
+
+    fn display(&self) -> impl fmt::Display;
 }
 
 fn event_for_file(name: &str) -> impl MatchEvent + '_ {
-    |event: &ChangeEvent| event.file_name() == Some(name)
+    MatchFile { name }
+}
+
+struct MatchFile<'a> {
+    name: &'a str,
+}
+
+impl MatchEvent for MatchFile<'_> {
+    fn match_event(&mut self, event: &ChangeEvent) -> bool {
+        event.file_name() == Some(self.name)
+    }
+
+    fn display(&self) -> impl Display {
+        format!("file name == {name}", name = self.name)
+    }
 }
 
 impl<F> MatchEvent for F
@@ -217,6 +241,10 @@
     fn match_event(&mut self, event: &ChangeEvent) -> bool {
         (*self)(event)
     }
+
+    fn display(&self) -> impl Display {
+        "<func>"
+    }
 }
 
 trait SetupFiles {
@@ -874,7 +902,7 @@
 
     std::fs::write(site_packages.join("a.py").as_std_path(), "class A: ...")?;
 
-    let changes = case.try_stop_watch(|_: &ChangeEvent| true, Duration::from_millis(100));
+    let changes = case.try_stop_watch(&mut |_: &ChangeEvent| true, Duration::from_millis(100));
 
     assert_eq!(changes, Err(vec![]));
 
```

---

_Comment by @zanieb on 2025-01-24 19:28_

That's pretty close to what I implemented! but I wasn't happy with `<func>`

---

_Comment by @MichaReiser on 2025-01-24 19:42_

> That's pretty close to what I implemented! but I wasn't happy with `<func>`

Most tests use the file name matcher. So it shouldn't be that big of a Problem 

---

_Comment by @zanieb on 2025-01-24 21:28_

Let's just ship this and consider that _next_ :D 

---

_Merged by @zanieb on 2025-01-24 21:28_

---

_Closed by @zanieb on 2025-01-24 21:28_

---

_Branch deleted on 2025-01-24 21:28_

---
