---
number: 242
title: Include error code and message in json output
type: issue
state: closed
author: harupy
labels: []
assignees: []
created_at: 2022-09-21T15:27:19Z
updated_at: 2022-09-23T00:29:22Z
url: https://github.com/astral-sh/ruff/issues/242
synced_at: 2026-01-07T13:12:14-06:00
---

# Include error code and message in json output

---

_Issue opened by @harupy on 2022-09-21 15:27_

It'd be nice if the JSON output includes error code and message.

### Current:

```
> ruff --format json resources/test/fixtures/F634.py
[
  {
    "kind": "IfTuple",
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  },
  ...
]
```

### Proposed:

```
> ruff --format json resources/test/fixtures/F634.py
[
  {
    "check": {
      "kind": "IfTuple",
      "code": "F123",
      "message": "If test is a tuple, which is always `True`"
    },
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  },
  ...
]
```

---

_Label `enhancement` added by @charliermarsh on 2022-09-21 15:30_

---

_Comment by @harupy on 2022-09-22 14:03_

Can I work on this?

---

_Comment by @charliermarsh on 2022-09-22 15:12_

Yeah, that'd be awesome!

---

_Comment by @harupy on 2022-09-22 16:35_

I'm making changes to include `code` and `body`. `git diff` looks like below. Is this the right way?

```diff
diff --git a/src/message.rs b/src/message.rs
index f273111..5ab0666 100644
--- a/src/message.rs
+++ b/src/message.rs
@@ -4,12 +4,13 @@ use std::path::Path;
 
 use colored::Colorize;
 use rustpython_parser::ast::Location;
+use serde::ser::{SerializeStruct, Serializer};
 use serde::{Deserialize, Serialize};
 
-use crate::checks::CheckKind;
+use crate::checks::{CheckCode, CheckKind};
 use crate::fs::relativize_path;
 
-#[derive(Debug, PartialEq, Eq, Serialize, Deserialize)]
+#[derive(Debug, PartialEq, Eq, Deserialize)]
 pub struct Message {
     pub kind: CheckKind,
     pub fixed: bool,
@@ -17,6 +18,32 @@ pub struct Message {
     pub filename: String,
 }
 
+#[derive(Serialize)]
+struct Check<'a> {
+    kind: &'a CheckKind,
+    code: &'a CheckCode,
+    body: &'a String,
+}
+
+impl Serialize for Message {
+    fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
+    where
+        S: Serializer,
+    {
+        let mut state = serializer.serialize_struct("Message", 4)?;
+        let check = Check {
+            kind: &self.kind,
+            code: &self.kind.code(),
+            body: &self.kind.body(),
+        };
+        state.serialize_field("check", &check)?;
+        state.serialize_field("fixed", &self.fixed)?;
+        state.serialize_field("location", &self.location)?;
+        state.serialize_field("filename", &self.filename)?;
+        state.end()
+    }
+}
+
 impl Ord for Message {
     fn cmp(&self, other: &Self) -> Ordering {
         (&self.filename, self.location.row(), self.location.column()).cmp(&(
```

```bash
> cargo run -- --format json resources/test/fixtures/F634.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff --format json resources/test/fixtures/F634.py`
[
  {
    "check": {
      "kind": "IfTuple",
      "code": "F634",
      "body": "If test is a tuple, which is always `True`"
    },
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  },
  {
    "check": {
      "kind": "IfTuple",
      "code": "F634",
      "body": "If test is a tuple, which is always `True`"
    },
    "fixed": false,
    "location": {
      "row": 7,
      "column": 5
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  }
]
```

---

_Comment by @harupy on 2022-09-22 16:48_

Another pattern:

```diff
diff --git a/src/message.rs b/src/message.rs
index f273111..3d8dea6 100644
--- a/src/message.rs
+++ b/src/message.rs
@@ -4,12 +4,13 @@ use std::path::Path;
 
 use colored::Colorize;
 use rustpython_parser::ast::Location;
+use serde::ser::{SerializeStruct, Serializer};
 use serde::{Deserialize, Serialize};
 
-use crate::checks::CheckKind;
+use crate::checks::{CheckCode, CheckKind};
 use crate::fs::relativize_path;
 
-#[derive(Debug, PartialEq, Eq, Serialize, Deserialize)]
+#[derive(Debug, PartialEq, Eq, Deserialize)]
 pub struct Message {
     pub kind: CheckKind,
     pub fixed: bool,
@@ -17,6 +18,22 @@ pub struct Message {
     pub filename: String,
 }
 
+impl Serialize for Message {
+    fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
+    where
+        S: Serializer,
+    {
+        let mut state = serializer.serialize_struct("Message", 6)?;
+        state.serialize_field("kind", &self.kind)?;
+        state.serialize_field("code", &self.kind.code())?;
+        state.serialize_field("message", &self.kind.body())?;
+        state.serialize_field("fixed", &self.fixed)?;
+        state.serialize_field("location", &self.location)?;
+        state.serialize_field("filename", &self.filename)?;
+        state.end()
+    }
+}
+
 impl Ord for Message {
     fn cmp(&self, other: &Self) -> Ordering {
         (&self.filename, self.location.row(), self.location.column()).cmp(&(
```

```
[
  {
    "kind": "IfTuple",
    "code": "F634",
    "message": "If test is a tuple, which is always `True`",
    "fixed": false,
    "location": {
      "row": 1,
      "column": 1
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  },
  {
    "kind": "IfTuple",
    "code": "F634",
    "message": "If test is a tuple, which is always `True`",
    "fixed": false,
    "location": {
      "row": 7,
      "column": 5
    },
    "filename": "/home/haru/Desktop/repositories/ruff/resources/test/fixtures/F634.py"
  }
]
```

---

_Comment by @charliermarsh on 2022-09-22 19:01_

Can you think of any way to implement this such that it doesn't affect the serialized results that we write to the cache, but instead, only the serialized representation that gets output by the JSON formatter? (Maybe we create a different struct, like `ExpandedMessage`, with all the fields we want, then map `Message` to `ExpandedMessage` and output _that_?)

---

_Comment by @harupy on 2022-09-22 23:56_

Mapping to `ExpandedMessage` sounds good!

---

_Referenced in [astral-sh/ruff#259](../../astral-sh/ruff/pulls/259.md) on 2022-09-23 00:20_

---

_Closed by @charliermarsh on 2022-09-23 00:29_

---
