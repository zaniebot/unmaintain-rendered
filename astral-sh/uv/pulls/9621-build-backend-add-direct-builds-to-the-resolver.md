```yaml
number: 9621
title: "Build backend: Add direct builds to the resolver and installer"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-resolver-direct-build2
created_at: 2024-12-03T21:44:05Z
updated_at: 2024-12-04T15:57:19Z
url: https://github.com/astral-sh/uv/pull/9621
synced_at: 2026-01-12T16:08:54Z
```

# Build backend: Add direct builds to the resolver and installer

---

_@konstin_

This is like #9556, but at the level of all other builds, including the resolver and installer. Going through PEP 517 to build a package is slow, so when building a package with the uv build backend, we can call into the uv build backend directly instead: No temporary virtual env, no temp venv sync, no python subprocess calls, no uv subprocess calls. 

This fast path is gated through preview. Since the uv wheel is not available at test time, I've manually confirmed the feature by comparing `uv venv && cargo run pip install . -v --preview --reinstall .` and `uv venv && cargo run pip install . -v --reinstall .`. When hacking the preview so that the python uv build backend works without the setting the direct build also (wheel built with `maturin build --profile profiling`), we can see the perfomance difference:

```
$ hyperfine --prepare "uv venv" --warmup 3 \
    "UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview" \
    "target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/"
Benchmark 1: UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview
  Time (mean ± σ):      33.1 ms ±   2.5 ms    [User: 25.7 ms, System: 13.0 ms]
  Range (min … max):    29.8 ms …  47.3 ms    73 runs
 
Benchmark 2: target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/
  Time (mean ± σ):     115.1 ms ±   4.3 ms    [User: 54.0 ms, System: 27.0 ms]
  Range (min … max):   109.2 ms … 123.8 ms    25 runs
 
Summary
  UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview ran
    3.48 ± 0.29 times faster than target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/
```

Do we need a global option to disable the fast path? There is one for `uv build` because `--force-pep517` moves `uv build` much closer to a `pip install` from source that a user of a library would experience (See discussion at #9610), but uv overall doesn't really make guarantees around the build env of dependencies, so I consider the direct build a valid option.

Best reviewed commit-by-commit, only the last commit is the actual implementation, while the preview mode introduction is just a refactoring touching too many files.

---

_Label `preview` added by @konstin on 2024-12-03 21:44_

---

_Review requested from @BurntSushi by @konstin on 2024-12-03 21:44_

---

_Review comment by @BurntSushi on `crates/uv-dispatch/src/lib.rs`:407 on 2024-12-04 13:27_

Should this be a `Option<&str>`?

---

_Review comment by @BurntSushi on `crates/uv-dispatch/src/lib.rs`:407 on 2024-12-04 13:30_

I see. It would be kinda annoying, but doable. I don't feel strongly.

---

_Review comment by @BurntSushi on `crates/uv-dispatch/src/lib.rs`:408 on 2024-12-04 13:30_

What is being returned here? And should it be a `PathBuf`?

---

_Review comment by @BurntSushi on `crates/uv-dispatch/src/lib.rs`:408 on 2024-12-04 13:32_

Oh okay, it's documented below on the trait.

---

_Review comment by @BurntSushi on `crates/uv-types/src/traits.rs`:129 on 2024-12-04 13:32_

What does it return in the case of an editable?

---

_@BurntSushi approved on 2024-12-04 13:33_

Nice, looks great!

---

_Comment by @BurntSushi on 2024-12-04 13:33_

Were you able to find any benchmarks showing an improvement? I guess they would be cold runs right?

---

_Comment by @konstin on 2024-12-04 15:07_

With hacking the preview so that the python uv build backend works without the setting the direct build also (wheel built with `maturin build --profile profiling`):

```
$ hyperfine --prepare "uv venv" --warmup 3 \
    "UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview" \
    "target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/"
Benchmark 1: UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview
  Time (mean ± σ):      33.1 ms ±   2.5 ms    [User: 25.7 ms, System: 13.0 ms]
  Range (min … max):    29.8 ms …  47.3 ms    73 runs
 
Benchmark 2: target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/
  Time (mean ± σ):     115.1 ms ±   4.3 ms    [User: 54.0 ms, System: 27.0 ms]
  Range (min … max):   109.2 ms … 123.8 ms    25 runs
 
Summary
  UV_PREVIEW=1 target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --preview ran
    3.48 ± 0.29 times faster than target/profiling/uv pip install --no-deps --reinstall scripts/packages/built-by-uv --find-links target/wheels/
```

---

_Review comment by @konstin on `crates/uv-types/src/traits.rs`:129 on 2024-12-04 15:14_

At this stage, the editable is a wheel that is indistinguishable from a regular wheel, except it contains a `.pth` file instead of the actual sources (but we still unpack it the regular way) and we're not allowed to cache it.

---

_@konstin reviewed on 2024-12-04 15:14_

---

_@konstin reviewed on 2024-12-04 15:22_

---

_Review comment by @konstin on `crates/uv-dispatch/src/lib.rs`:408 on 2024-12-04 15:22_

This initially happened because PEP 517 says that the build backend returns the filename as last line, and we passed that around as a string.

---

_@konstin reviewed on 2024-12-04 15:23_

---

_Review comment by @konstin on `crates/uv-dispatch/src/lib.rs`:407 on 2024-12-04 15:23_

It's more converting back and forth now, but it's also move consistent with the rest of the codebase.

---

_@konstin reviewed on 2024-12-04 15:28_

---

_Review comment by @konstin on `crates/uv-dispatch/src/lib.rs`:408 on 2024-12-04 15:28_

More specifically, the location on disk may not be normalized the same as `WheelFilename` is, so for non-uv build backends we have to pass out a string and only parse later, i added comments.

---

_Merged by @konstin on 2024-12-04 15:57_

---

_Closed by @konstin on 2024-12-04 15:57_

---

_Branch deleted on 2024-12-04 15:57_

---
