```yaml
number: 8424
title: Switch from a git snapshot to released rust-netrc 1.1.2
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
assignees: []
merged: true
base: main
head: netrc-1.1.2
created_at: 2024-10-21T18:32:48Z
updated_at: 2024-10-21T18:43:08Z
url: https://github.com/astral-sh/uv/pull/8424
synced_at: 2026-01-10T12:54:09Z
```

# Switch from a git snapshot to released rust-netrc 1.1.2

---

_Pull request opened by @musicinmybrain on 2024-10-21 18:32_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Before this PR (and since 651fe6f4e60c0d3a789c7593c4285817d6adf932) `uv` depends on a git snapshot of `rust-netrc` at https://github.com/gribouille/netrc/commit/544f3890b621f0dc30fcefb4f804269c160ce2e9, with fixes from https://github.com/gribouille/netrc/pull/3 for https://github.com/astral-sh/uv/issues/8003.

Since `rust-netrc` 0.1.2 was just released, and it includes those fixes – plus an additional [change to support `~`-expansion](https://github.com/gribouille/netrc/commit/ca0860c0a07274a156dfbd4aa686eee86fe9a0ed) – `uv` can go back to depending on published crates from crates.io.

## Test Plan

<!-- How was it tested? -->

```
$ cargo build
$ cargo run python install
$ cargo test
```

I get

```

────────────────────────────────────────────────────────────────────────────────

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: sha
Source: crates/uv/tests/it/build.rs:1454
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
Expression: snapshot
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   11    11 │ writing top-level names to src/project.egg-info/top_level.txt
   12    12 │ writing manifest file 'src/project.egg-info/SOURCES.txt'
   13    13 │ reading manifest file 'src/project.egg-info/SOURCES.txt'
   14    14 │ writing manifest file 'src/project.egg-info/SOURCES.txt'
         15 │+[CACHE_DIR]/builds-v0/[TMP]/pkg_resources.html
         16 │+  import pkg_resources
   15    17 │ running sdist
   16    18 │ running egg_info
   17    19 │ writing src/project.egg-info/PKG-INFO
   18    20 │ writing dependency_links to src/project.egg-info/dependency_links.txt
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   35    37 │ copying src/project.egg-info/top_level.txt -> project-0.1.0/src/project.egg-info
   36    38 │ Writing project-0.1.0/setup.cfg
   37    39 │ Creating tar archive
   38    40 │ removing 'project-0.1.0' (and everything under it)
         41 │+[CACHE_DIR]/builds-v0/[TMP]/pkg_resources.html
         42 │+  import pkg_resources
   39    43 │ Building wheel from source distribution...
   40    44 │ running egg_info
   41    45 │ writing src/project.egg-info/PKG-INFO
   42    46 │ writing dependency_links to src/project.egg-info/dependency_links.txt
   43    47 │ writing requirements to src/project.egg-info/requires.txt
   44    48 │ writing top-level names to src/project.egg-info/top_level.txt
   45    49 │ reading manifest file 'src/project.egg-info/SOURCES.txt'
   46    50 │ writing manifest file 'src/project.egg-info/SOURCES.txt'
         51 │+[CACHE_DIR]/builds-v0/[TMP]/pkg_resources.html
         52 │+  import pkg_resources
   47    53 │ running bdist_wheel
   48    54 │ running build
   49    55 │ running build_py
   50    56 │ creating build
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   73    79 │ adding 'project-0.1.0.dist-info/WHEEL'
   74    80 │ adding 'project-0.1.0.dist-info/top_level.txt'
   75    81 │ adding 'project-0.1.0.dist-info/RECORD'
   76    82 │ removing build/bdist.linux-x86_64/wheel
         83 │+[CACHE_DIR]/builds-v0/[TMP]/pkg_resources.html
         84 │+  import pkg_resources
   77    85 │ Successfully built dist/project-0.1.0.tar.gz and dist/project-0.1.0-py3-none-any.whl
────────────┴───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'build::sha' panicked at /home/ben/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.40.0/src/runtime.rs:548:9:
snapshot assertion for 'sha' failed in line 1454
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    build::sha

test result: FAILED. 1299 passed; 1 failed; 4 ignored; 0 measured; 0 filtered out; finished in 101.18s

error: test failed, to rerun pass `-p uv --test it`
```

The sole failure looks unrelated to me, and I can reproduce it on `main` (currently e8b8daf0fb055a310fae523cbeefea2048884476), so I conclude that it has nothing to do with this change.

---

_Comment by @zanieb on 2024-10-21 18:34_

Thank you!

---

_@charliermarsh approved on 2024-10-21 18:42_

---

_Label `internal` added by @charliermarsh on 2024-10-21 18:42_

---

_Merged by @charliermarsh on 2024-10-21 18:43_

---

_Closed by @charliermarsh on 2024-10-21 18:43_

---
