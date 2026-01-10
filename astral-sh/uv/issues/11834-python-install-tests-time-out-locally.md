```yaml
number: 11834
title: Python install tests time out locally
type: issue
state: open
author: konstin
labels:
  - internal
assignees: []
created_at: 2025-02-27T15:26:41Z
updated_at: 2025-02-27T18:28:01Z
url: https://github.com/astral-sh/uv/issues/11834
synced_at: 2026-01-10T03:50:31Z
```

# Python install tests time out locally

---

_Issue opened by @konstin on 2025-02-27 15:26_

When running the full test suite locally with nextest, I usually get timeouts in the `python_install` tests, e.g.

* `python_install::python_install_freethreaded`
* `python_install::python_install_preview_upgrade`
* `python_install::python_reinstall`

The test suite is network-bound for me, so i think this is the python downloads taking too long.

---

_Label `internal` added by @konstin on 2025-02-27 15:26_

---

_Comment by @zanieb on 2025-02-27 15:51_

As previously noted, we should install these from a persistent cache and allow it in tests so we can go fast here.

---

_Comment by @konstin on 2025-02-27 16:24_

Where would the caching mechanism for those files live?

---

_Comment by @zanieb on 2025-02-27 18:09_

I think we'd use it in production, as a new cache bucket.

---

_Comment by @zanieb on 2025-02-27 18:10_

xref

- https://github.com/astral-sh/uv/issues/8025

---

_Comment by @konstin on 2025-02-27 18:13_

Does that mean that in the tests, we would only check the cache bucket -> installed Python path? Or does this only apply locally, but not in CI?

---

_Comment by @zanieb on 2025-02-27 18:28_

> Does that mean that in the tests, we would only check the cache bucket -> installed Python path? 

Yes. We could have some tests that specifically skip the cache, but we don't need it for every test case since we're mostly covering install behavior. I want the install behavior tests to be faster locally and in CI.

---
