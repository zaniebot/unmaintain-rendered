```yaml
number: 14458
title: Log the debug error trace
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/log-debug-error
created_at: 2025-07-04T09:26:00Z
updated_at: 2025-07-31T17:13:12Z
url: https://github.com/astral-sh/uv/pull/14458
synced_at: 2026-01-12T16:11:13Z
```

# Log the debug error trace

---

_@konstin_

For #14425. We can see the error without `error(transparent)` applied by looking at the debug representation.

---

_Label `internal` added by @konstin on 2025-07-04 09:26_

---

_Comment by @zanieb on 2025-07-04 13:38_

Should this be `trace!`?

---

_Comment by @konstin on 2025-07-24 10:04_

I changed it to a trace log.

---

_Comment by @zanieb on 2025-07-24 11:57_

Do you feel like we still need this? I think things are working better now?

---

_Comment by @konstin on 2025-07-31 15:12_

I think #14996 is a good indicator that we still need tooling to debug (network) errors.

---

_Comment by @zanieb on 2025-07-31 15:16_

What's an example of the output?

---

_Comment by @konstin on 2025-07-31 15:35_

Something that's not really represented here is that we're also showing `error(transparent)` steps that are otherwise hidden. The inner anyhow error here makes the formatting odd. 

```
TRACE Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple/tqdm/", query: None, fragment: None }, WrappedReqwestError(Middleware(Request failed after 3 retries

Caused by:
    0: error sending request for url (https://pypi.org/simple/tqdm/)
    1: client error (Connect)
    2: dns error
    3: failed to lookup address information: Name or service not known))) }
```

In previous cases with only a generic IO error as output, I had to guess the origin of the error. Similarly, sometimes two variants on two enums have the same string representation. Having the exact derivation narrows this down, especially for error we can't reproduce locally.

The limitation is that it only works if we're in a thiserror chain, it fails when only anyhow is involved:

```
TRACE Error trace: failed to remove file `/home/konsti/projects/uv/.venv/lib/python3.13/site-packages/anyio/abc/__init__.py`: Permission denied (os error 13)
```

---

_Comment by @zanieb on 2025-07-31 15:44_

This will leak credentials from the `DisplaySafeUrl`, right?

---

_Comment by @konstin on 2025-07-31 16:20_

Good point to consider, but I don't think so?

https://github.com/astral-sh/uv/blob/4dd039208661f9aefc1500079e7f2598c435da37/crates/uv-redacted/src/lib.rs#L147-L176

---

_Comment by @zanieb on 2025-07-31 16:26_

Oh cool, thanks for checking!

---

_@zanieb approved on 2025-07-31 16:26_

---

_Merged by @konstin on 2025-07-31 17:13_

---

_Closed by @konstin on 2025-07-31 17:13_

---

_Branch deleted on 2025-07-31 17:13_

---
