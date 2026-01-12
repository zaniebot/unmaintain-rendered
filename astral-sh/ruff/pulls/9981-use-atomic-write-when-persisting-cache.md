```yaml
number: 9981
title: Use atomic write when persisting cache
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: atomic-cache-write
created_at: 2024-02-14T09:56:30Z
updated_at: 2024-02-14T14:09:22Z
url: https://github.com/astral-sh/ruff/pull/9981
synced_at: 2026-01-12T15:55:30Z
```

# Use atomic write when persisting cache

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/8147

I ~strongly suspect~ have prove that the cache gets corrupted because of a race between different ruff processes writing to the same cache file but with different content (e.g. format writes the cache with the "old" lint results and lint updates the lint results).
Multiple processes writing to the same cache file is possible because POSIX only guarantees that a single `write` call is atomic, but our 
implementation uses a `BufWriter` that chunks the data into multiple write calls if necessary. 

This PR changes our `persist` implementation to use a temporary file instead and renames it on success. Renaming is guaranteed
to be atomic. This approach has the added benefit of preventing cache corruption if ruff dies while writing the cache data (SIGKIL, panic, the computer shuts down...). 

This PR removes the `BufWriter` because I noticed that the implementation became slower when using a temporary file, and the `BufWriter` (I added the [recommended `flush`](https://doc.rust-lang.org/std/io/struct.BufWriter.html) call to the `BufWriter`).
Removing the `BufWriter` gives us about the same performance for the CPython benchmark with the default rules but results in a ~2% speedup when selecting all rules (instead of a 5% slowdown due to the use of a tempfile). 

## Test Plan

I wrote a script to reproduce my theory ~~but failed to get a single reproduction~~. 

The script starts ten ruff instances in a loop with the default or all rules (coinflip). The instances must use different rules or all instances write the same cache file, which makes it impossible to show the race.  ~~But without success. Ruff never fails with the old build :(~~ I had to patch up the cache to a) use the same cache regardless of the settings, and b) never return cached data but always write to the cache. 

This allowed me to reproduce the bug fairly consistently on main. I'm no longer able to reproduce the issue with the changes from this PR:

<details>

```javascript
const child_process = require("child_process");

async function run() {
  for (let i = 0; i < 100; ++i) {
    let promises = [];
    for (let i = 0; i < 10; ++i) {
      const rules =
        Math.random() > 0.5
          ? ["--select", "ALL"]
          : ["--select", "ALL", "--ignore", "ANN001"];
      //   console.log(rules);
      promises.push(
        spawn("../../ruff/target/release/ruff", ["check", ".", "-s", ...rules])
      );
    }

    let results = await Promise.all(promises);

    for (let result of results) {
      process.stdout.write(result[0]);
      process.stderr.write(result[1]);
    }
  }
}

function spawn(cmd, args) {
  return new Promise((resolve, reject) => {
    const cp = child_process.spawn(cmd, args);
    const stderr = [];
    const stdout = [];
    cp.stdout.on("data", (data) => {
      stdout.push(data.toString());
    });

    cp.on("error", (e) => {
      stderr.push(e.toString());
    });

    cp.on("close", () => {
      if (stderr.length) reject(stderr.join(""));
      else resolve([stdout.join(""), stderr.join("")]);
    });
  });
}

run().catch((error) => console.error(error));
```

```diff
Subject: [PATCH] isort-lines-after-imports
---
Index: crates/ruff/src/cache.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff/src/cache.rs b/crates/ruff/src/cache.rs
--- a/crates/ruff/src/cache.rs	(revision 297a6ffcae54b5dd4fd7c1d7d5a94f6b305ad607)
+++ b/crates/ruff/src/cache.rs	(date 1707913224871)
@@ -364,7 +364,7 @@
 fn cache_key(package_root: &Path, settings: &Settings) -> u64 {
     let mut hasher = CacheKeyHasher::new();
     package_root.cache_key(&mut hasher);
-    settings.cache_key(&mut hasher);
+    // settings.cache_key(&mut hasher);
 
     hasher.finish()
 }
Index: crates/ruff/src/diagnostics.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff/src/diagnostics.rs b/crates/ruff/src/diagnostics.rs
--- a/crates/ruff/src/diagnostics.rs	(revision 297a6ffcae54b5dd4fd7c1d7d5a94f6b305ad607)
+++ b/crates/ruff/src/diagnostics.rs	(date 1707913224871)
@@ -200,19 +200,19 @@
             let cached_diagnostics = cache
                 .get(relative_path, &cache_key)
                 .and_then(|entry| entry.to_diagnostics(path));
-            if let Some(diagnostics) = cached_diagnostics {
-                // `FixMode::Generate` and `FixMode::Diff` rely on side-effects (writing to disk,
-                // and writing the diff to stdout, respectively). If a file has diagnostics, we
-                // need to avoid reading from and writing to the cache in these modes.
-                if match fix_mode {
-                    flags::FixMode::Generate => true,
-                    flags::FixMode::Apply | flags::FixMode::Diff => {
-                        diagnostics.messages.is_empty() && diagnostics.fixed.is_empty()
-                    }
-                } {
-                    return Ok(diagnostics);
-                }
-            }
+            // if let Some(diagnostics) = cached_diagnostics {
+            //     // `FixMode::Generate` and `FixMode::Diff` rely on side-effects (writing to disk,
+            //     // and writing the diff to stdout, respectively). If a file has diagnostics, we
+            //     // need to avoid reading from and writing to the cache in these modes.
+            //     if match fix_mode {
+            //         flags::FixMode::Generate => true,
+            //         flags::FixMode::Apply | flags::FixMode::Diff => {
+            //             diagnostics.messages.is_empty() && diagnostics.fixed.is_empty()
+            //         }
+            //     } {
+            //         return Ok(diagnostics);
+            //     }
+            // }
 
             // Stash the file metadata for later so when we update the cache it reflects the prerun
             // information

```
</details>

## Benchmarks

```
❯ hyperfine --warmup 10 --runs 100 \
        "./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e" \
        "./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e"
Benchmark 1: ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e
  Time (mean ± σ):      37.4 ms ±   0.7 ms    [User: 44.2 ms, System: 54.0 ms]
  Range (min … max):    35.3 ms …  39.1 ms    100 runs
 
Benchmark 2: ./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e
  Time (mean ± σ):      37.6 ms ±   0.8 ms    [User: 43.9 ms, System: 55.0 ms]
  Range (min … max):    36.0 ms …  40.2 ms    100 runs
 
Summary
  ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e ran
    1.01 ± 0.03 times faster than ./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e

ruff on  atomic-cache-write [$!] is 📦 v0.2.1 via 🐍 v3.11.7 via 🦀 v1.76.0 took 8s 
❯ hyperfine --warmup 10 --runs 20 \
        "./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e --select=ALL" \
        "./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e --select=ALL"
Benchmark 1: ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e --select=ALL
  Time (mean ± σ):     617.6 ms ±  10.4 ms    [User: 849.8 ms, System: 300.7 ms]
  Range (min … max):   593.8 ms … 635.7 ms    20 runs
 
Benchmark 2: ./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e --select=ALL
  Time (mean ± σ):     601.8 ms ±  14.4 ms    [User: 823.6 ms, System: 320.5 ms]
  Range (min … max):   576.3 ms … 631.0 ms    20 runs
 
Summary
  ./target/release/ruff-atomic check crates/ruff_linter/resources/test/cpython -e --select=ALL ran
    1.03 ± 0.03 times faster than ./target/release/ruff-main check crates/ruff_linter/resources/test/cpython -e --select=ALL
```


---

_Label `bug` added by @MichaReiser on 2024-02-14 09:58_

---

_Label `cli` added by @MichaReiser on 2024-02-14 10:00_

---

_Comment by @github-actions[bot] on 2024-02-14 10:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-14 12:24_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-14 12:25_

---

_@BurntSushi approved on 2024-02-14 13:30_

Nice find and fix!

---

_@charliermarsh approved on 2024-02-14 13:54_

Excellent!

---

_Comment by @MichaReiser on 2024-02-14 14:09_

> Excellent!

I knew that would make you happy ;) This bug annoyed you for a long time.

---

_Merged by @MichaReiser on 2024-02-14 14:09_

---

_Closed by @MichaReiser on 2024-02-14 14:09_

---

_Branch deleted on 2024-02-14 14:09_

---
