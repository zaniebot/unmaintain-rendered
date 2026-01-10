```yaml
number: 14174
title: Lock the source tree when running setuptools, to protect concurrent builds
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: main
head: jack/lock_wheel_builds
created_at: 2025-06-20T23:30:35Z
updated_at: 2025-06-26T19:28:17Z
url: https://github.com/astral-sh/uv/pull/14174
synced_at: 2026-01-10T11:10:43Z
```

# Lock the source tree when running setuptools, to protect concurrent builds

---

_Pull request opened by @oconnor663 on 2025-06-20 23:30_

Fixes https://github.com/astral-sh/uv/issues/13703.

If I repeat the repro steps from [my comment on that issue](https://github.com/astral-sh/uv/issues/13703#issuecomment-2993000368) with this PR, the errors are gone:

```
$ cat >setup.py <<EOF
import setuptools
setuptools.setup(name="scratch", version="4")
EOF
$ for i in `seq 10` ; do
    ~/uv/target/fast-build/uv build --wheel &
done
[...lots of build output, no errors...]
```

There's also no performance impact:

```
$ hyperfine "/tmp/before build --wheel" "/tmp/after build --wheel"
Benchmark 1: /tmp/before build --wheel
  Time (mean ± σ):     255.8 ms ±   3.0 ms    [User: 225.7 ms, System: 41.5 ms]
  Range (min … max):   251.4 ms … 260.5 ms    11 runs

Benchmark 2: /tmp/after build --wheel
  Time (mean ± σ):     255.3 ms ±   2.6 ms    [User: 219.7 ms, System: 47.3 ms]
  Range (min … max):   252.8 ms … 262.0 ms    11 runs

Summary
  /tmp/after build --wheel ran
    1.00 ± 0.02 times faster than /tmp/before build --wheel
```

---

_Review requested from @zanieb by @oconnor663 on 2025-06-20 23:30_

---

_Review requested from @konstin by @oconnor663 on 2025-06-20 23:30_

---

_Comment by @charliermarsh on 2025-06-21 00:18_

I don't think locking the _output_ directory is actually sufficient because IIRC setuptools uses a build directory in the _source_ directory? So, like, concurrent builds to different output directories would still fail?

---

_Comment by @oconnor663 on 2025-06-21 00:41_

Ah true, I still get failures with something like this:

```
$ for i in `seq 10` ; do
    ~/uv/target/fast-build/uv build --wheel --out-dir /tmp/mybuild$i &
done
```

---

_Comment by @oconnor663 on 2025-06-23 21:50_

I've switched to locking (the hash of) the source dir. The following now succeeds with no errors, and the builds run in series:

```
$ cat >setup.py <<EOF
import setuptools
setuptools.setup(name="scratch", version="4")
EOF
$ for i in `seq 10` ; do
    ~/uv/target/fast-build/uv build --wheel --out-dir `mktemp -d` &
done
```

I'm considering refactoring this a bit to stop putting so many file locks directly in /tmp. Going to put another PR on top of this one, which we can take or not.

---

_Renamed from "Make `uv build` lock its output dir, to prevent concurrent builds from interfering" to "Make `uv build` lock its source dir, to prevent concurrent builds from interfering" by @oconnor663 on 2025-06-23 22:08_

---

_Comment by @oconnor663 on 2025-06-23 22:08_

@zanieb suggested that special-casing `setuptools` might make more sense than taking a lock under `/tmp`. I'm going to look into that. (In that case we could still choose to take https://github.com/astral-sh/uv/pull/14225, but it would no longer be related to this RP.)

---

_Renamed from "Make `uv build` lock its source dir, to prevent concurrent builds from interfering" to "Lock the source tree when running setuptools, to protect concurrent builds" by @oconnor663 on 2025-06-25 18:40_

---

_Comment by @oconnor663 on 2025-06-25 18:43_

Just put up a new iteration. I've kept the same locking strategy (a `cache_digest`-named file under `temp_dir()`), but I've made it specific to setuptools, and we take the lock before we invoke any setuptools commands, including `get_requires_for...`. I noticed that even that read-only-sounding command creates `*.egg-info`, and even though I haven't seen that lead to failures when multiple processes do it at once, it seems safer to protect it. @konstin do you have time to take a look?

---

_@oconnor663 reviewed on 2025-06-25 18:45_

---

_Review comment by @oconnor663 on `crates/uv-build-frontend/src/lib.rs`:763 on 2025-06-25 18:45_

These are drive-by cleanup.

---

_@konstin approved on 2025-06-26 13:06_

This makes sense to me

---

_@charliermarsh approved on 2025-06-26 14:17_

---

_Comment by @charliermarsh on 2025-06-26 14:18_

I'm not a huge fan of special-casing setuptools but I don't feel strongly enough to advocate for a change.

---

_Comment by @charliermarsh on 2025-06-26 14:18_

(Actually just ignore my comment, it's alright.)

---

_Comment by @oconnor663 on 2025-06-26 17:03_

(rebasing to pick up test fixes from main)

---

_Merged by @oconnor663 on 2025-06-26 19:28_

---

_Closed by @oconnor663 on 2025-06-26 19:28_

---

_Branch deleted on 2025-06-26 19:28_

---
