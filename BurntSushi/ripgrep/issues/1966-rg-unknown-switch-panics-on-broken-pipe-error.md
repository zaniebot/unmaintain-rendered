```yaml
number: 1966
title: "`rg --unknown-switch` panics on broken pipe error"
type: issue
state: closed
author: lopopolo
labels:
  - bug
  - rollup
assignees: []
created_at: 2021-08-06T19:58:17Z
updated_at: 2023-11-21T04:51:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1966
synced_at: 2026-01-12T16:13:24Z
```

# `rg --unknown-switch` panics on broken pipe error

---

_@lopopolo_

#### What version of ripgrep are you using?

```console
$ ./target/debug/rg --version
ripgrep 13.0.0 (rev 9b01a8f9ae)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Compiled from source.

```console
$ git rev-parse HEAD
9b01a8f9ae53ebcd05c27ec21843758c2c1e823f
```

#### What operating system are you using ripgrep on?

macOS Big Sur 11.5.1 (20G80)

#### Describe your bug.

ripgrep panics when printing a clap error to stderr if stderr is closed.

#### What are the steps to reproduce the behavior?

Apply this diff to ripgrep:

```diff
diff --git a/crates/core/main.rs b/crates/core/main.rs
index 47385de..f3d9889 100644
--- a/crates/core/main.rs
+++ b/crates/core/main.rs
@@ -46,10 +46,14 @@ static ALLOC: jemallocator::Jemalloc = jemallocator::Jemalloc;
 type Result<T> = ::std::result::Result<T, Box<dyn error::Error>>;

 fn main() {
-    if let Err(err) = Args::parse().and_then(try_main) {
-        eprintln!("{}", err);
-        process::exit(2);
-    }
+    let f = std::panic::catch_unwind(std::panic::AssertUnwindSafe(|| {
+        if let Err(err) = Args::parse().and_then(try_main) {
+            eprintln!("{}", err);
+            process::exit(2);
+        }
+    }));
+    println!("panicked? - {}", f.is_err());
+    process::exit(1);
 }

 fn try_main(args: Args) -> Result<()> {
```

Then:

```console
$ cargo build
$ ./target/debug/rg --unknown-switch 3>&1 1>&2 2>&3 | head -c 0 # swap stdout and stderr, close stdout
head: illegal byte count -- 0
panicked? - true
$ echo $?
1
```

I'm not sure how I could observe this panic without the patch since rust writes the panic to stderr. Maybe if ripgrep is compiled with `panic = abort`?

#### What is the actual behavior?

ripgrep panics if stderr is closed despite going to great lengths to avoid panicking when writing to stdout.

#### What is the expected behavior?

ripgrep does not panic.


---

_Comment by @lopopolo on 2021-08-06 20:01_

I discovered this in my own project after trying to match ripgrep's panic free printing of `--help` text from clap: https://github.com/artichoke/artichoke/issues/1314.

---

_Label `rollup` added by @BurntSushi on 2023-11-21 00:01_

---

_Label `bug` added by @BurntSushi on 2023-11-21 00:02_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
