```yaml
number: 15314
title: Allow pinning managed Python versions to specific build versions
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - configuration
assignees: []
merged: true
base: main
head: zb/download-build-key
created_at: 2025-08-15T19:24:15Z
updated_at: 2025-08-25T21:25:07Z
url: https://github.com/astral-sh/uv/pull/15314
synced_at: 2026-01-10T06:44:33Z
```

# Allow pinning managed Python versions to specific build versions

---

_Pull request opened by @zanieb on 2025-08-15 19:24_

Allows pinning the Python build version via environment variables, e.g., `UV_PYTHON_CPYTHON_BUILD=...`. Each variable is implementation specific, because they use different versioning schemes.

Updates the Python download metadata to include a `build` string, so we can filter downloads by the pin. Writes the build version to a file in the managed install, e.g., `cpython-3.10.18-macos-aarch64-none/BUILD`, so we can filter installed versions by the pin.

Some important follow-up here:

- Include the build version in not found errors (when pinned)
- Automatically use a remote list of Python downloads to satisfy build versions not present in the latest embedded download metadata

Some less important follow-ups to consider:

- Allow using ranges for build version pins

---

_Label `enhancement` added by @zanieb on 2025-08-16 14:16_

---

_Label `configuration` added by @zanieb on 2025-08-16 14:16_

---

_Marked ready for review by @zanieb on 2025-08-20 20:30_

---

_@zanieb reviewed on 2025-08-20 20:31_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:222 on 2025-08-20 20:31_

Absolutely nightmarish to get the filters working on Windows and Unix. Hopefully this makes them more robust elsewhere.

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:832 on 2025-08-20 22:56_

I want to make use of this for some error messages to avoid confusion when this is in a request. I'm not sure what the best way to do so is yet though.

---

_@zanieb reviewed on 2025-08-20 22:56_

---

_Review comment by @konstin on `crates/uv-python/src/python_version.rs`:20 on 2025-08-21 13:20_

This should not use anyhow

---

_Review comment by @konstin on `crates/uv-python/src/python_version.rs`:238 on 2025-08-21 13:21_

We should use `env::var` here, this also avoids using our own error type

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:40 on 2025-08-21 13:25_

nit: join these two lines

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:1811 on 2025-08-21 13:37_

Doesn't that mean we change what we test over time, depending on how or when we re-release? Should we assert that it matches something, and bump the build if it would match nothing anymore?

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:1864 on 2025-08-21 13:40_

Why are those imports here and not top level?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:352 on 2025-08-21 13:42_

We should not leak, we can use a `Cow` for this.

---

_Review comment by @konstin on `crates/uv-python/src/python_version.rs`:223 on 2025-08-21 13:43_

Should this be a method on implementation name?

---

_@konstin reviewed on 2025-08-21 13:45_

---

_@zanieb reviewed on 2025-08-21 13:58_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:352 on 2025-08-21 13:58_

This matches the existing pattern — I definitely didn't come up with this :D

---

_@zanieb reviewed on 2025-08-21 13:59_

---

_Review comment by @zanieb on `crates/uv-python/src/python_version.rs`:223 on 2025-08-21 13:59_

It could be 🤔 I wanted to keep all this logic together but I don't feel strongly.

---

_@zanieb reviewed on 2025-08-21 14:01_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:1811 on 2025-08-21 14:01_

Yeah this one could change. We should use a build number that matches an older patch version because that won't change.

---

_@zanieb reviewed on 2025-08-21 14:04_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:1811 on 2025-08-21 14:04_

Honestly this test is silly. I might replace this entirely.

---

_@zanieb reviewed on 2025-08-21 14:07_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:352 on 2025-08-21 14:07_

It was done in https://github.com/astral-sh/uv/pull/10939

---

_@zanieb reviewed on 2025-08-21 14:09_

---

_Review comment by @zanieb on `crates/uv-python/src/python_version.rs`:20 on 2025-08-21 14:09_

Thanks, that was silly.

---

_@zanieb reviewed on 2025-08-21 14:14_

---

_Review comment by @zanieb on `crates/uv-python/src/python_version.rs`:238 on 2025-08-21 14:14_

I don't want to use `env::var` here, it will error if not present and I want that to have different behavior. I also want the dedicated error type for when we perform further validation of these values in the future, e.g., we know what a well formed build version looks like.

---

_@konstin reviewed on 2025-08-21 14:24_

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:352 on 2025-08-21 14:24_

This breaks this the pattern in the rest of the code https://github.com/astral-sh/uv/pull/15418

---

_@konstin reviewed on 2025-08-21 14:26_

---

_Review comment by @konstin on `crates/uv-python/src/python_version.rs`:238 on 2025-08-21 14:26_

It makes sense if we add our own validation logic on top.

---

_@zanieb reviewed on 2025-08-21 15:11_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:352 on 2025-08-21 15:11_

I'm all for using `Cow`, it seems better.

---

_Merged by @zanieb on 2025-08-25 21:25_

---

_Closed by @zanieb on 2025-08-25 21:25_

---

_Branch deleted on 2025-08-25 21:25_

---
