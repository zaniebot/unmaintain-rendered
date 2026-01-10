```yaml
number: 16780
title: "CI Perf: fast-build"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cargo-fast-build
created_at: 2025-11-19T18:03:07Z
updated_at: 2025-12-11T13:51:39Z
url: https://github.com/astral-sh/uv/pull/16780
synced_at: 2026-01-10T05:49:14Z
```

# CI Perf: fast-build

---

_Pull request opened by @konstin on 2025-11-19 18:03_

_No description provided._

---

_Label `internal` added by @konstin on 2025-11-19 18:03_

---

_Comment by @konstin on 2025-11-19 18:55_

Windows seems unchanged, Linux goes from 6:xx min to 4:xx min

---

_Comment by @zanieb on 2025-11-19 19:20_

Can we use this for the binary builds we use for integration tests too?

Is there risk to using these fast builds for tests? Are we going to lose test coverage?

---

_Comment by @konstin on 2025-11-19 19:27_

The fast-build profile strip debug info, so we'd lose information if we running a coverage tools like coverage.py, but I'm not aware that we're using such a tool. No debug info also means that if there's a crash, we don't get a backtrace, similar to our release builds. If that's a good tradeoff IMHO depends on how much faster it is.

We don't use any functional coverage in our tests though, the binaries are only a bit more optimized and stripped of debuginfo, basically half way between a regular debug builds and full release builds.

---

_Comment by @konstin on 2025-11-19 19:28_

> Can we use this for the binary builds we use for integration tests too?

Added, let's see how it goes


---

_Comment by @konstin on 2025-11-28 09:49_

The optimal profile is different for tests vs. for binary builds: For tests, we want opt level 1 and no debug info, while for binary builds, we only want no debug info, opt level 1 makes the builds slower (e.g. 45s to 1min 15s).

LLM'd plot:

<img width="2085" height="1777" alt="image" src="https://github.com/user-attachments/assets/152c6807-2917-492f-be2c-31ab25854d95" />

Another run, different plotting:

<img width="2085" height="1777" alt="image" src="https://github.com/user-attachments/assets/482926f0-3578-4142-a4e9-06fea0d53426" />




---

_Marked ready for review by @konstin on 2025-11-28 09:50_

---

_@zanieb reviewed on 2025-12-04 17:46_

---

_Review comment by @zanieb on `README.md`:199 on 2025-12-04 17:46_

```suggestion
 + cpython-3.12.12-macos-aarch64-none (python3.12)
 + cpython-3.13.9-macos-aarch64-none (python3.13)
 + cpython-3.14.0-macos-aarch64-none (python3.14)
```

---

_Review comment by @zanieb on `Cargo.toml`:317 on 2025-12-04 17:47_

If it's oriented for tests should we call it `profile.testing`? or similar?

---

_@zanieb reviewed on 2025-12-04 17:47_

---

_@zanieb approved on 2025-12-04 17:47_

---

_@konstin reviewed on 2025-12-05 15:37_

---

_Review comment by @konstin on `Cargo.toml`:317 on 2025-12-05 15:37_

Yes, I only wanted to avoid the churn here.

---

_Merged by @zanieb on 2025-12-11 13:51_

---

_Closed by @zanieb on 2025-12-11 13:51_

---

_Branch deleted on 2025-12-11 13:51_

---
