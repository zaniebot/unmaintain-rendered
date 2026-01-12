```yaml
number: 6643
title: Detect musl and error for musl pbs builds
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - musl
assignees: []
merged: true
base: main
head: konsti/musl-detection
created_at: 2024-08-26T12:47:08Z
updated_at: 2024-08-27T00:06:54Z
url: https://github.com/astral-sh/uv/pull/6643
synced_at: 2026-01-12T16:07:27Z
```

# Detect musl and error for musl pbs builds

---

_@konstin_

As described in #4242, we're currently incorrectly downloading glibc python-build-standalone on musl target, but we also can't fix this by using musl python-build-standalone on musl targets since the musl builds are effectively broken.

We reintroduce the libc detection previously removed in #2381, using it to detect which libc is the current one before we have a python interpreter. I changed the strategy a big to support an empty `PATH` which we use in the tests.

For simplicity, i've decided to just filter out the musl python-build-standalone archives from the list of available archive, given this is temporary. This means we show the same error message as if we don't have a build for the platform. We could also add a dedicated error message for musl.

Fixes #4242

## Test Plan

Tested manually.

On my ubuntu host, python downloads continue to pass:
```
target/x86_64-unknown-linux-musl/debug/uv python install
```

On alpine, we fail:
```
$ docker run -it --rm -v .:/io alpine /io/target/x86_64-unknown-linux-musl/debug/uv python install
  Searching for Python installations
  error: No download found for request: cpython-any-linux-x86_64-musl
```

---

_Review requested from @zanieb by @konstin on 2024-08-26 12:47_

---

_Label `bug` added by @konstin on 2024-08-26 12:47_

---

_Label `musl` added by @konstin on 2024-08-26 12:47_

---

_@zanieb approved on 2024-08-26 18:08_

---

_Comment by @zanieb on 2024-08-26 18:08_

Can you add a note to https://docs.astral.sh/uv/concepts/python-versions/#managed-python-distributions

---

_Comment by @charliermarsh on 2024-08-27 00:00_

Added docs, merging...

---

_Merged by @charliermarsh on 2024-08-27 00:06_

---

_Closed by @charliermarsh on 2024-08-27 00:06_

---

_Branch deleted on 2024-08-27 00:06_

---
