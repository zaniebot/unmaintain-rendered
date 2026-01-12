```yaml
number: 2110
title: Better offline error message
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/better-offline-error-message
created_at: 2024-03-01T12:24:58Z
updated_at: 2024-03-04T14:47:41Z
url: https://github.com/astral-sh/uv/pull/2110
synced_at: 2026-01-12T16:04:52Z
```

# Better offline error message

---

_@konstin_

Error for `uv pip compile scripts/requirements/jupyter.in` without internet:

**Before**

```
error: error sending request for url (https://pypi.org/simple/jupyter/): error trying to connect: dns error: failed to lookup address information: No such host is known. (os error 11001)
  Caused by: error trying to connect: dns error: failed to lookup address information: No such host is known. (os error 11001)
  Caused by: dns error: failed to lookup address information: No such host is known. (os error 11001)
  Caused by: failed to lookup address information:  No such host is known. (os error 11001)
```

**After**

```
error: Could not connect, are you offline?
  Caused by: error sending request for url (https://pypi.org/simple/django/): error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: error trying to connect: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
```

On linux, it would be "Temporary failure in name resolution" instead of "No such host is known. (os error 11001)".

The implementation checks for "dne error" stringly as hyper errors are opaque. The danger is that this breaks with a hyper update. We still get the complete error trace since reqwest eagerly inlines errors (https://github.com/seanmonstar/reqwest/issues/2147).

No test since i wouldn't know how to simulate this in cargo test.

Fixes #1971


---

_Label `error messages` added by @konstin on 2024-03-01 12:24_

---

_Review requested from @charliermarsh by @konstin on 2024-03-01 12:24_

---

_Comment by @zanieb on 2024-03-01 15:48_

Isn't the error / caused by out of order here?

---

_Comment by @konstin on 2024-03-01 16:51_

Yeah, will switch. The feature i really want is to attach hints to error messages that are shown below the error chain themselves. I also want that for build errors (#2108). We could do that by defining our own `ErrorHint` trait and downcasting when we print the chain in main.

---

_Comment by @konstin on 2024-03-04 11:03_

I've switched the error hierarchy, it's verbose again.

I've experimented a bit with adding help messages but i don't think it's possible without either adding that trait to our entire error hierarchy (the miette approach) or losing the error type in a `struct ErrorWithHelp { help: String, source: Box<dyn Error + Send + Sync + 'static> }`.

---

_Review request for @charliermarsh removed by @konstin on 2024-03-04 11:03_

---

_Review requested from @zanieb by @konstin on 2024-03-04 11:03_

---

_@zanieb approved on 2024-03-04 14:40_

---

_Merged by @konstin on 2024-03-04 14:47_

---

_Closed by @konstin on 2024-03-04 14:47_

---

_Branch deleted on 2024-03-04 14:47_

---
