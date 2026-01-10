```yaml
number: 9556
title: "Build backend: Add fast path "
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-fast-path
created_at: 2024-12-01T20:39:09Z
updated_at: 2024-12-03T16:50:32Z
url: https://github.com/astral-sh/uv/pull/9556
synced_at: 2026-01-10T12:00:00Z
```

# Build backend: Add fast path 

---

_Pull request opened by @konstin on 2024-12-01 20:39_

Going through PEP 517 to build a package is slow, so when building a package with the uv build backend, we can call into the uv build backend directly. This is the basis for the `uv build --list`.

This does not enable the fast path for general source dependencies.

There is a possible difference in execution if the latest uv version is newer than the one currently running: The PEP 517 path would use the latest version, while the fast path uses the current version.

Please review commit-by-commit

### Benchmark

`built_with_uv`, using the fast path:
```
$ hyperfine "~/projects/uv/target/profiling/uv build"
Time (mean ± σ):       9.2 ms ±   1.1 ms    [User: 4.6 ms, System: 4.6 ms]
Range (min … max):     6.4 ms …  12.7 ms    290 runs
```

`hatcling_editable`, with hatchling being optimized for fast startup times:
```
$ hyperfine "~/projects/uv/target/profiling/uv build"
Time (mean ± σ):     270.5 ms ±  18.4 ms    [User: 230.8 ms, System: 44.5 ms]
Range (min … max):   250.7 ms … 298.4 ms    10 runs
```

---

_Label `preview` added by @konstin on 2024-12-01 20:39_

---

_Review requested from @BurntSushi by @konstin on 2024-12-01 20:39_

---

_Review requested from @charliermarsh by @konstin on 2024-12-01 20:39_

---

_Comment by @charliermarsh on 2024-12-01 21:09_

This is sick!

---

_@charliermarsh reviewed on 2024-12-01 22:24_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2187 on 2024-12-01 22:24_

I think we should give this a more descriptive name, like `--use-external-uv` or something. Since we might add other "fast paths" in the future. What do you think?

---

_@charliermarsh reviewed on 2024-12-01 22:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:742 on 2024-12-01 22:26_

I might structure these as just two entirely separate methods? That's always my instinct when I have a method that takes a bool and entirely forks behavior on that bool. Sort of a matter of preference, though.

---

_@charliermarsh approved on 2024-12-01 22:26_

---

_@konstin reviewed on 2024-12-01 22:37_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:2187 on 2024-12-01 22:37_

Good idea. I thought about something like `--pep517`, maybe `--force-pep517`? 

---

_@konstin reviewed on 2024-12-01 22:38_

---

_Review comment by @konstin on `crates/uv/src/commands/build_frontend.rs`:742 on 2024-12-01 22:38_

I like those, in this case i already know that I'll add more forks (listing vs. building) here later :)

---

_@charliermarsh reviewed on 2024-12-01 23:03_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2187 on 2024-12-01 23:03_

`--force-pep517` seems reasonable... 

---

_Merged by @konstin on 2024-12-02 15:37_

---

_Closed by @konstin on 2024-12-02 15:37_

---

_Branch deleted on 2024-12-02 15:37_

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:2187 on 2024-12-03 14:24_

Ref #9600

---

_@BurntSushi reviewed on 2024-12-03 14:25_

Nice! The benchmark shows a rather large perf difference. Do you have a sense of where this is coming from?

And that also makes me curious about the perf difference between the fast path and `--force-pep517`. That seems like a more apples-to-apples comparison? (Unless I'm misunderstanding the benchmark in the PR description.)

---

_Comment by @konstin on 2024-12-03 16:25_

For all the python build backends, we pay a cost for each import (see also https://peps.python.org/pep-0690/). A build backend is using a lot of features, so it gets slowed down by a lot of imports, even something manually optimized to have lazy imports such as hatchling.

For uv-against-uv, we paying the cost for setting up the build env, syncing it, spawning python, the spawning uv from python. We do this for each hook we call (should be `build_sdist` and `build_wheel` here).

Here's a uv-against-uv benchmark:

```
UV_PREVIEW=1 hyperfine "../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview" "UV_PREVIEW=1 ../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview --force-pep517"
Benchmark 1: ../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview
  Time (mean ± σ):       9.7 ms ±   1.7 ms    [User: 4.7 ms, System: 4.9 ms]
  Range (min … max):     6.5 ms …  21.7 ms    265 runs
 
Benchmark 2: UV_PREVIEW=1 ../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview --force-pep517
  Time (mean ± σ):      87.7 ms ±   4.2 ms    [User: 61.0 ms, System: 28.4 ms]
  Range (min … max):    79.6 ms …  98.5 ms    33 runs
 
Summary
  ../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview ran
    9.08 ± 1.61 times faster than UV_PREVIEW=1 ../../../target/profiling/uv build --find-links ../../../target/wheels/ --preview --force-pep517
```

To illustrate the overhead from calling python, this is the cost for some std modules you'll want for building distributions:

```
$ hyperfine "python -c ''"
Benchmark 1: python -c ''
  Time (mean ± σ):       7.9 ms ±   1.2 ms    [User: 5.9 ms, System: 2.0 ms]
  Range (min … max):     6.2 ms …  10.4 ms    383 runs
$ hyperfine "python -c 'import sys'"
Benchmark 1: python -c 'import sys'
  Time (mean ± σ):       8.1 ms ±   1.2 ms    [User: 5.9 ms, System: 2.1 ms]
  Range (min … max):     6.3 ms …  10.7 ms    342 runs
$ hyperfine "python -c 'import sys, os, subprocess, pathlib, zipfile'"
Benchmark 1: python -c 'import sys, os, subprocess, pathlib, zipfile'
  Time (mean ± σ):      17.5 ms ±   2.1 ms    [User: 14.3 ms, System: 3.1 ms]
  Range (min … max):    15.1 ms …  24.6 ms    170 runs
```

The uv build backend imports `sys` and `subprocess` lazily, and has to spawn uv, so we can look at their isolated overhead:

```
$ UV_PREVIEW=1 hyperfine "python -c 'import sys, subprocess; from uv import build_wheel'"
Benchmark 1: python -c 'import sys, subprocess; from uv import build_wheel'
  Time (mean ± σ):      14.7 ms ±   1.8 ms    [User: 11.8 ms, System: 2.9 ms]
  Range (min … max):    12.4 ms …  20.2 ms    219 runs
$ hyperfine -N "uv build-backend --version"
Benchmark 1: uv build-backend --version
  Time (mean ± σ):       2.4 ms ±   0.3 ms    [User: 0.9 ms, System: 1.5 ms]
  Range (min … max):     1.6 ms …   3.1 ms    968 runs
```

---

_Comment by @BurntSushi on 2024-12-03 16:50_

Wow. So cool.

---
