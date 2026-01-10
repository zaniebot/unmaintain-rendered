```yaml
number: 4090
title: "Fix tracing output in `cargo dev` commands"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/cargo-dev
created_at: 2024-06-06T12:05:42Z
updated_at: 2024-06-06T12:56:27Z
url: https://github.com/astral-sh/uv/pull/4090
synced_at: 2026-01-10T13:54:02Z
```

# Fix tracing output in `cargo dev` commands

---

_Pull request opened by @zanieb on 2024-06-06 12:05_

There's probably a more succinct way to do this, but I figure we should have it be similar to `uv::main` so I copied most of the content from there.

Now `cargo dev fetch-python` output is shown again.

---

_Label `internal` added by @zanieb on 2024-06-06 12:05_

---

_Review requested from @konstin by @zanieb on 2024-06-06 12:23_

---

_Comment by @konstin on 2024-06-06 12:37_

Do you want the same logging infrastructure in uv-dev as in uv or do you just want basic logging in `cargo dev fetch-python`?

---

_Comment by @zanieb on 2024-06-06 12:39_

I don't really care, I just want logging to work again. I spent like 20 minutes thinking the command wasn't working. I figure having to be similar to `uv` but a bit stripped down makes sense. If someone wants something else I'm fine with tha ttoo. 

---

_Comment by @konstin on 2024-06-06 12:46_

The change below does basic logging. I want to avoid duplicating this code because it means having to fix any logging bug twice. We can put the logging code into a function and call it from both main functions (`uv-dev` can depend on `uv`, i think), or we say basic logging is enough in uv-dev and do the below.

```diff
diff --git a/crates/uv-dev/src/main.rs b/crates/uv-dev/src/main.rs
index d3a3b723..75b19dbc 100644
--- a/crates/uv-dev/src/main.rs
+++ b/crates/uv-dev/src/main.rs
@@ -1,6 +1,7 @@
 use std::env;
 use std::path::PathBuf;
 use std::process::ExitCode;
+use std::str::FromStr;
 use std::time::Instant;
 
 use anstream::{eprintln, println};
@@ -10,9 +11,10 @@ use owo_colors::OwoColorize;
 use tracing::{debug, instrument};
 use tracing_durations_export::plot::PlotConfig;
 use tracing_durations_export::DurationsLayerBuilder;
+use tracing_subscriber::filter::Directive;
 use tracing_subscriber::layer::SubscriberExt;
 use tracing_subscriber::util::SubscriberInitExt;
-use tracing_subscriber::EnvFilter;
+use tracing_subscriber::{EnvFilter, Layer};
 
 use crate::build::{build, BuildArgs};
 use crate::clear_compile::ClearCompileArgs;
@@ -117,15 +119,21 @@ async fn main() -> ExitCode {
         (None, None)
     };
 
-    let filter_layer = EnvFilter::try_from_default_env().unwrap_or_else(|_| {
-        EnvFilter::builder()
-            // Show only the important spans
-            .parse("uv_dev=info,uv_dispatch=info")
-            .unwrap()
-    });
+    // Show `INFO` messages from the `uv` crate, but allow `RUST_LOG` to override.
+    let default_directive = Directive::from_str("uv=info").unwrap();
+
+    let filter = EnvFilter::builder()
+        .with_default_directive(default_directive)
+        .from_env()
+        .expect("Valid RUST_LOG directives");
+
     tracing_subscriber::registry()
         .with(duration_layer)
-        .with(filter_layer)
+        .with(
+            tracing_subscriber::fmt::layer()
+                .with_writer(std::io::stderr)
+                .with_filter(filter),
+        )
         .init();
 
     let start = Instant::now();
```


---

_Comment by @zanieb on 2024-06-06 12:56_

@konstin want to just open a PR for that then?

---

_Closed by @zanieb on 2024-06-06 12:56_

---
