```yaml
number: 5775
title: Ignore directories when collecting files to lint
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: exclude-folder-ending-with-py
created_at: 2023-07-15T11:19:14Z
updated_at: 2023-07-18T01:37:13Z
url: https://github.com/astral-sh/ruff/pull/5775
synced_at: 2026-01-12T03:30:21Z
```

# Ignore directories when collecting files to lint

---

_Pull request opened by @harupy on 2023-07-15 11:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #5739

## Test Plan

<!-- How was it tested? -->

Manually tested:

```sh
$ tree dir
dir
├── dir.py
│   └── file.py
└── file.py

1 directory, 2 files

$ cargo run -p ruff_cli -- check dir --no-cache
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/ruff check dir --no-cache`
dir/dir.py/file.py:1:7: F821 Undefined name `a`
dir/file.py:1:7: F821 Undefined name `a`
Found 2 errors.
```

Is a unit test needed?

---

_Review comment by @harupy on `crates/ruff/src/resolver.rs`:332 on 2023-07-15 11:32_

Used a variable to avoid https://github.com/astral-sh/ruff/actions/runs/5561952462/jobs/10159951956

---

_@harupy reviewed on 2023-07-15 11:32_

---

_Comment by @github-actions[bot] on 2023-07-15 11:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.03ms     4.1 MB/sec    1.00      9.9±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1899.3±3.32µs     8.8 MB/sec    1.00   1881.3±2.85µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    206.3±0.70µs    14.3 MB/sec    1.01   208.9±29.13µs    14.1 MB/sec
formatter/pydantic/types.py                1.02      4.2±0.01ms     6.0 MB/sec    1.00      4.1±0.00ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.00     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    381.7±2.96µs     7.7 MB/sec    1.00    378.7±1.24µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.05ms     4.0 MB/sec    1.01      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.17ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1460.4±3.41µs    11.4 MB/sec    1.00   1455.6±2.31µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.0±1.11µs    18.8 MB/sec    1.00    155.5±0.17µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.1 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.06ms     3.6 MB/sec    1.00     11.2±0.42ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.04ms     7.8 MB/sec    1.00      2.1±0.02ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    231.8±2.62µs    12.7 MB/sec    1.00    231.3±5.17µs    12.8 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.05ms     5.4 MB/sec    1.00      4.7±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.08ms     2.6 MB/sec    1.00     15.7±0.08ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.1±5.07µs     6.9 MB/sec    1.00    426.5±6.32µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.01ms     5.1 MB/sec    1.01      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1643.3±9.74µs    10.1 MB/sec    1.01  1657.5±34.87µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.4±1.67µs    16.4 MB/sec    1.00    179.2±2.03µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-15 14:01_

Thanks for contributing @harupy!

Do you think you could write a unit test for this? 

---

_Review comment by @zanieb on `crates/ruff/src/resolver.rs`:332 on 2023-07-15 14:02_

I believe you can also avoid the clippy lint and retain the old pattern by not using `return` in your statement, see https://github.com/astral-sh/ruff/commit/c9b305e9cadbcc9af465643ba84ce13b460fc48d

---

_@zanieb reviewed on 2023-07-15 14:02_

---

_@harupy reviewed on 2023-07-15 14:12_

---

_Review comment by @harupy on `crates/ruff/src/resolver.rs`:332 on 2023-07-15 14:12_

@zanieb Thanks! Updated

---

_Comment by @charliermarsh on 2023-07-15 14:55_

Unfortunately we don’t have any unit tests for the file discovery behavior right now, so we’d need to establish some new patterns. 

We have some “manual” tests that are document expected behavior here: https://github.com/astral-sh/ruff/blob/main/crates/ruff/resources/test/project/README.md.

Alternatively we could add an integration test? Those can rely on the binary which would make the test itself easier to write (but the output harder to introspect).

I’m comfortable with any outcome including just doing some manual testing since it’s “no worse than before”.

---

_Comment by @charliermarsh on 2023-07-15 14:57_

(The logic looks correct to me.)

---

_Comment by @harupy on 2023-07-15 15:29_

@charliermarsh Thanks for the comment! I'm trying to write a unit test. My local diff looks like this:

```diff
diff --git a/Cargo.lock b/Cargo.lock
index f3ce5f85e..428d00996 100644
--- a/Cargo.lock
+++ b/Cargo.lock
@@ -1893,6 +1893,7 @@ dependencies = [
  "smallvec",
  "strum",
  "strum_macros",
+ "tempfile",
  "test-case",
  "thiserror",
  "toml",
diff --git a/crates/ruff/Cargo.toml b/crates/ruff/Cargo.toml
index b659730d8..8d22661c1 100644
--- a/crates/ruff/Cargo.toml
+++ b/crates/ruff/Cargo.toml
@@ -83,6 +83,7 @@ wsl = { version = "0.1.0" }
 [dev-dependencies]
 insta = { workspace = true }
 pretty_assertions = "1.3.0"
+tempfile = "3.6.0"
 test-case = { workspace = true }
 # Disable colored output in tests
 colored = { workspace = true, features = ["no-color"] }
diff --git a/crates/ruff/src/resolver.rs b/crates/ruff/src/resolver.rs
index ccbd7ecc3..3f3cc39c5 100644
--- a/crates/ruff/src/resolver.rs
+++ b/crates/ruff/src/resolver.rs
@@ -435,10 +435,12 @@ mod tests {
     use anyhow::Result;
     use globset::GlobSet;
     use path_absolutize::Absolutize;
+    use tempfile::TempDir;
+    use test_case::test_case;
 
     use crate::resolver::{
-        is_file_excluded, match_exclusion, resolve_settings_with_processor, NoOpProcessor,
-        PyprojectConfig, PyprojectDiscoveryStrategy, Relativity, Resolver,
+        is_file_excluded, match_exclusion, python_files_in_path, resolve_settings_with_processor,
+        NoOpProcessor, PyprojectConfig, PyprojectDiscoveryStrategy, Relativity, Resolver,
     };
     use crate::settings::pyproject::find_settings_toml;
     use crate::settings::types::FilePattern;
@@ -605,4 +607,37 @@ mod tests {
         ));
         Ok(())
     }
+
+    #[test_case("py")]
+    #[test_case("pyi")]
+    #[test_case("ipynb")]
+    fn python_files_in_path_excludes_directories_that_look_like_python_files(
+        ext: &str,
+    ) -> Result<()> {
+        let tmp_dir = TempDir::new().unwrap();
+        let tmp_dir = tmp_dir.path();
+        let file = tmp_dir.join(format!("file.{}", ext));
+        let dir = tmp_dir.join(format!("dir.{}", ext));
+        let another_dir = tmp_dir.join(format!("another_dir.{}", ext));
+        let another_file = dir.join(format!("another_file.{}", ext));
+        std::fs::create_dir(&dir).unwrap();
+        std::fs::create_dir(&another_dir).unwrap();
+        std::fs::File::create(&file).unwrap();
+        std::fs::File::create(&another_file).unwrap();
+        let files = python_files_in_path(
+            &[tmp_dir.to_path_buf()],
+            &PyprojectConfig::new(
+                PyprojectDiscoveryStrategy::Hierarchical,
+                Default::default(),
+                None,
+            ),
+            &NoOpProcessor,
+        )
+        .unwrap()
+        .0;
+        assert_eq!(files.len(), 2);
+        assert_eq!(files[0].as_ref().unwrap().path(), another_file);
+        assert_eq!(files[1].as_ref().unwrap().path(), file);
+        Ok(())
+    }
 }
```


---

_Comment by @harupy on 2023-07-15 23:13_

@charliermarsh I manually ran the commands in https://github.com/astral-sh/ruff/blob/main/crates/ruff/resources/test/project/README.md. Here are the results.

```
> cargo run -p ruff_cli -- check crates/ruff/resources/test/project/
   Compiling ruff_python_semantic v0.0.0 (/home/haru/Desktop/repositories/ruff/crates/ruff_python_semantic)
   Compiling ruff_python_formatter v0.0.0 (/home/haru/Desktop/repositories/ruff/crates/ruff_python_formatter)
   Compiling ruff v0.0.278 (/home/haru/Desktop/repositories/ruff/crates/ruff)
   Compiling ruff_cli v0.0.278 (/home/haru/Desktop/repositories/ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 11.34s
     Running `target/debug/ruff check crates/ruff/resources/test/project/`
warning: Detected debug build without --no-cache.
crates/ruff/resources/test/project/examples/.dotfiles/script.py:1:1: I001 [*] Import block is un-sorted or un-formatted
crates/ruff/resources/test/project/examples/.dotfiles/script.py:1:17: F401 [*] `numpy` imported but unused
crates/ruff/resources/test/project/examples/.dotfiles/script.py:2:17: F401 [*] `app.app_file` imported but unused
crates/ruff/resources/test/project/examples/docs/docs/file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
crates/ruff/resources/test/project/examples/docs/docs/file.py:8:5: F841 [*] Local variable `x` is assigned to but never used
crates/ruff/resources/test/project/project/file.py:1:8: F401 [*] `os` imported but unused
crates/ruff/resources/test/project/project/import_file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 7 errors.
[*] 7 potentially fixable with the --fix option.
```

```
> (cd crates/ruff/resources/test/project/ && cargo run -p ruff_cli -- check .)
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `/home/haru/Desktop/repositories/ruff/target/debug/ruff check .`
warning: Detected debug build without --no-cache.
examples/.dotfiles/script.py:1:1: I001 [*] Import block is un-sorted or un-formatted
examples/.dotfiles/script.py:1:17: F401 [*] `numpy` imported but unused
examples/.dotfiles/script.py:2:17: F401 [*] `app.app_file` imported but unused
examples/docs/docs/file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
examples/docs/docs/file.py:8:5: F841 [*] Local variable `x` is assigned to but never used
project/file.py:1:8: F401 [*] `os` imported but unused
project/import_file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 7 errors.
[*] 7 potentially fixable with the --fix option.
```

```
> (cd crates/ruff/resources/test/project/examples/docs && cargo run -p ruff_cli -- check .)
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `/home/haru/Desktop/repositories/ruff/target/debug/ruff check .`
warning: Detected debug build without --no-cache.
docs/file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
docs/file.py:8:5: F841 [*] Local variable `x` is assigned to but never used
Found 2 errors.
[*] 2 potentially fixable with the --fix option.
```

```
> (cargo run -p ruff_cli -- check --config=crates/ruff/resources/test/project/pyproject.toml crates/ruff/resources/test/project/)
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff check --config=crates/ruff/resources/test/project/pyproject.toml crates/ruff/resources/test/project/`
warning: Detected debug build without --no-cache.
crates/ruff/resources/test/project/examples/.dotfiles/script.py:1:17: F401 [*] `numpy` imported but unused
crates/ruff/resources/test/project/examples/.dotfiles/script.py:2:17: F401 [*] `app.app_file` imported but unused
crates/ruff/resources/test/project/examples/docs/docs/concepts/file.py:1:8: F401 [*] `os` imported but unused
crates/ruff/resources/test/project/examples/docs/docs/file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
crates/ruff/resources/test/project/examples/docs/docs/file.py:1:8: F401 [*] `os` imported but unused
crates/ruff/resources/test/project/examples/docs/docs/file.py:3:17: F401 [*] `numpy` imported but unused
crates/ruff/resources/test/project/examples/docs/docs/file.py:4:27: F401 [*] `docs.concepts.file` imported but unused
crates/ruff/resources/test/project/examples/excluded/script.py:1:8: F401 [*] `os` imported but unused
crates/ruff/resources/test/project/project/file.py:1:8: F401 [*] `os` imported but unused
Found 9 errors.
[*] 9 potentially fixable with the --fix option.
```

```
> (cd crates/ruff/resources/test/project/examples && cargo run -p ruff_cli -- check --config=docs/ruff.toml .)
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `/home/haru/Desktop/repositories/ruff/target/debug/ruff check --config=docs/ruff.toml .`
warning: Detected debug build without --no-cache.
docs/docs/concepts/file.py:5:5: F841 [*] Local variable `x` is assigned to but never used
docs/docs/file.py:1:1: I001 [*] Import block is un-sorted or un-formatted
docs/docs/file.py:8:5: F841 [*] Local variable `x` is assigned to but never used
excluded/script.py:5:5: F841 [*] Local variable `x` is assigned to but never used
Found 4 errors.
[*] 4 potentially fixable with the --fix option.
```

```
> cargo run -p ruff_cli -- check crates/ruff/resources/test/project/examples/excluded/
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff check crates/ruff/resources/test/project/examples/excluded/`
warning: Detected debug build without --no-cache.
crates/ruff/resources/test/project/examples/excluded/script.py:1:8: F401 [*] `os` imported but unused
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

```
> cargo run -p ruff_cli -- check crates/ruff/resources/test/project/examples/excluded/ --force-exclude
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/ruff check crates/ruff/resources/test/project/examples/excluded/ --force-exclude`
warning: Detected debug build without --no-cache.
warning: No Python files found under the given path(s)
```

```
> cargo run -p ruff_cli -- check crates/ruff/resources/test/project/examples/excluded/ --force-exclude
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/ruff check crates/ruff/resources/test/project/examples/excluded/ --force-exclude`
warning: Detected debug build without --no-cache.
warning: No Python files found under the given path(s)
```

---

_Review requested from @zanieb by @harupy on 2023-07-16 00:58_

---

_Comment by @charliermarsh on 2023-07-16 01:01_

I like where you're headed with that test, but I'd also be fine to add tests in a future PR for all of this behavior holistically rather than trying to figure out the right pattern for this bug fix. I defer to @zanieb as first reviewer :)

---

_Comment by @harupy on 2023-07-16 01:19_

> I'd also be fine to add tests in a future PR for all of this behavior holistically rather than trying to figure out the right pattern for this bug fix.

+1

---

_Comment by @harupy on 2023-07-18 01:13_

@zanieb Can you take another look?

---

_@zanieb approved on 2023-07-18 01:25_

Thank you!

I confirmed this worked locally as well.
#5850 will track unit test patterns for file discovery. Feel free to add more functionality there that we should add test coverage for.

---

_Merged by @zanieb on 2023-07-18 01:25_

---

_Closed by @zanieb on 2023-07-18 01:25_

---

_Branch deleted on 2023-07-18 01:37_

---
