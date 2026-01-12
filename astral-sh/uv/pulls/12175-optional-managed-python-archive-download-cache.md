```yaml
number: 12175
title: Optional managed Python archive download cache
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/cache-python-download-archives
created_at: 2025-03-14T19:19:37Z
updated_at: 2025-04-28T10:09:11Z
url: https://github.com/astral-sh/uv/pull/12175
synced_at: 2026-01-12T16:10:09Z
```

# Optional managed Python archive download cache

---

_@konstin_

Part of #11834

Currently, all Python installation are a streaming download-and-extract. With this PR, we add the `UV_PYTHON_CACHE_DIR` variable. When set, the installation is split into downloading the interpreter into `UV_PYTHON_CACHE_DIR` and extracting it there from a second step. If the archive is already present in `UV_PYTHON_CACHE_DIR`, we skip the download.

The feature can be used to speed up tests and CI. Locally for me, `cargo test -p uv -- python_install` goes from 43s to 7s (1,7s in release mode) when setting `UV_PYTHON_CACHE_DIR`. It can also be used for offline installation of Python interpreter, by copying the archives to a directory in the offline machine, while the path rewriting is still performed on the target machine on installation.

---

_Label `enhancement` added by @konstin on 2025-03-14 19:19_

---

_Marked ready for review by @konstin on 2025-03-19 14:47_

---

_Review requested from @zanieb by @konstin on 2025-03-19 14:48_

---

_@zanieb reviewed on 2025-03-19 18:19_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:997 on 2025-03-19 18:19_

I find it a bit surprising to call it `PythonBuilds` since we're not performing a build there? I'd prefer `Python` or `PythonVersions`

---

_@zanieb reviewed on 2025-03-19 18:21_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:635 on 2025-03-19 18:21_

As in, compatibility with file systems? If so, can you add that for clarity? I wasn't sure what we were trying to be compatible with here.

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:790 on 2025-03-19 18:24_

Isn't this into any target directory? Why temporary here?

---

_@zanieb reviewed on 2025-03-19 18:24_

---

_Comment by @zanieb on 2025-03-19 18:30_

@Gankra could you give this a look too? I'm curious if you have thoughts on the streaming question.

@konstin I'm still not sure of the benefits of storing the compressed archives instead of an unpacked one and tweaking the install logic. Immediately unpacking would solve the streaming problem as well as the delayed hash check (which feels awkward here). It'd also help with the automatic install user experience (ref https://github.com/astral-sh/uv/issues/12122#issuecomment-2718673044) — I think writing a marker file isn't unreasonable but it's harder to reason about when viewing the directories and may be annoying when we start bundling Python distributions into "virtual" environments.

---

_@Gankra approved on 2025-03-19 19:23_

re: stream teeing -- hard to imagine what exactly is up with the borrowchecks without using it in anger but my experiences here are similarly dire frustration so I wouldn't dwell too hard on it, especially since the wins will be marginal compared to how big of a win you've got already.

---

_Comment by @konstin on 2025-03-26 21:30_

Could we only have one unpacked tree instead of two unpacked trees, e.g. with (out of band) tracking whether a Python interpreter as installed? I was targeting fixing the test running with this change, so tackling our Python installation behavior in general would change the scope quite a bit. Moving more features such as decompression out of the test path would take test coverage of an important path in the test setup (vs. just the download, which is already a well-tested logic), so I'd prefer to not cache too much outside the case. We could also go the other way and decrease the scope of this PR by only acting on the environment variable that we only use in testing and only then use the two-step split download first, decompress-and-installer later.

---

_Comment by @zanieb on 2025-03-26 21:39_

> Could we only have one unpacked tree instead of two unpacked trees, 

Are you considering one unpacked cache tree and another hard or soft linked tree as two unpacked trees?

> Moving more features such as decompression out of the test path would take test coverage of an important path in the test setup (vs. just the download, which is already a well-tested logic)

I think we'll want at least one test case that does not use this new cache.

---

_Comment by @konstin on 2025-03-27 19:46_

> > Could we only have one unpacked tree instead of two unpacked trees,
> 
> Are you considering one unpacked cache tree and another hard or soft linked tree as two unpacked trees?

Sounds great, but is that this change or a different PR, i.e. would that behavior change fix the download behavior?

> > Moving more features such as decompression out of the test path would take test coverage of an important path in the test setup (vs. just the download, which is already a well-tested logic)
> 
> I think we'll want at least one test case that does not use this new cache.

Sounds good.

---

_Comment by @zanieb on 2025-04-16 16:41_

I don't recall where we landed on this during the in-person discussion. Do you? :D

---

_Renamed from "Cache managed Python archive downloads" to "Optional managed Python archive download cache" by @konstin on 2025-04-25 09:59_

---

_@konstin reviewed on 2025-04-25 10:21_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:635 on 2025-04-25 10:21_

Yes, clarified

---

_@konstin reviewed on 2025-04-25 10:26_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:790 on 2025-04-25 10:26_

We use a temporary directory first, then do an atomic rename.

---

_Comment by @konstin on 2025-04-25 10:40_

We decided to change this change to be only about cases where `UV_PYTHON_CACHE_DIR` is set. This allows both speeding up the cache and makes it easier to install managed Python on offline machines. Handling whether a Python interpreter was explicitly (`uv python install`) or implicitly (`uv run -p 3.x` with a missing python version) installed is separated from this change.

---

_@zanieb approved on 2025-04-25 18:32_

---

_Merged by @konstin on 2025-04-28 10:09_

---

_Closed by @konstin on 2025-04-28 10:09_

---

_Branch deleted on 2025-04-28 10:09_

---
