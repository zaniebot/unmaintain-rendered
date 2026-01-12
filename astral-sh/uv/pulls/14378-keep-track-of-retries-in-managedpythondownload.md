```yaml
number: 14378
title: "Keep track of retries in `ManagedPythonDownload::fetch_with_retry`"
type: pull_request
state: merged
author: oconnor663
labels:
  - enhancement
assignees: []
merged: true
base: main
head: jack/track_retries
created_at: 2025-06-30T21:54:34Z
updated_at: 2025-07-03T22:12:47Z
url: https://github.com/astral-sh/uv/pull/14378
synced_at: 2026-01-12T16:11:11Z
```

# Keep track of retries in `ManagedPythonDownload::fetch_with_retry`

---

_@oconnor663_

If/when we see https://github.com/astral-sh/uv/issues/14171 again, this should clarify whether our retry logic was skipped (i.e. a transient error wasn't correctly identified as transient), or whether we exhausted our retries. Previously, if you ran a local example fileserver as in https://github.com/astral-sh/uv/issues/14171#issuecomment-3014580701 and then you tried to install Python from it, you'd get:

```
$ export UV_TEST_NO_CLI_PROGRESS=1
$ uv python install 3.8.20 --mirror http://localhost:8000 2>&1 | cat
error: Failed to install cpython-3.8.20-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.8.20-20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/jacko/.local/share/uv/python/.temp/.tmpS4sHHZ/python/lib/libpython3.8.so.1.0`
  Caused by: failed to unpack `python/lib/libpython3.8.so.1.0` into `/home/jacko/.local/share/uv/python/.temp/.tmpS4sHHZ/python/lib/libpython3.8.so.1.0`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
```

With this change you get:

```
error: Failed to install cpython-3.8.20-linux-x86_64-gnu
  Caused by: Request failed after 3 retries
  Caused by: Failed to extract archive: cpython-3.8.20-20241002-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/jacko/.local/share/uv/python/.temp/.tmp4Ia24w/python/lib/libpython3.8.so.1.0`
  Caused by: failed to unpack `python/lib/libpython3.8.so.1.0` into `/home/jacko/.local/share/uv/python/.temp/.tmp4Ia24w/python/lib/libpython3.8.so.1.0`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: Connection reset by peer (os error 104)
```

At the same time, I'm updating the way we handle the retry count to avoid nested retry loops exceeding the intended number of attempts, as I mentioned at https://github.com/astral-sh/uv/issues/14069#issuecomment-3020634281. It's not clear to me whether we actually want this part of the change, and I need feedback here.

---

_Review comment by @oconnor663 on `crates/uv-python/src/downloads.rs`:124 on 2025-06-30 21:55_

The fragility here makes me feel like I'm doing something wrong. @konstin do you have any better ideas?

---

_@oconnor663 reviewed on 2025-06-30 21:55_

---

_Label `enhancement` added by @oconnor663 on 2025-06-30 23:26_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:124 on 2025-07-01 09:27_

iirc streaming error can show up as different error types (e.g. `Error::ExtractError`), but we're only attaching the retries if we have a reqwest error

---

_@konstin reviewed on 2025-07-01 09:27_

---

_Marked ready for review by @oconnor663 on 2025-07-01 15:42_

---

_@konstin approved on 2025-07-01 15:42_

---

_@oconnor663 reviewed on 2025-07-01 15:52_

---

_Review comment by @oconnor663 on `crates/uv-python/src/downloads.rs`:124 on 2025-07-01 15:52_

Yeah if opening the connection (the inner loop) retries a couple times and then ultimately succeeds, we'll never see those counts. Konsti and I talked about it and decided that it's not too big of a problem. It is confusing for folks reading the code and trying to understand how many retries are possible, but it's unlikely that it will bother any users in practice.

---

_Comment by @oconnor663 on 2025-07-01 16:28_

Note sure about [this CI failure](https://github.com/astral-sh/uv/actions/runs/15984494035/job/45147781382?pr=14378):

```
  Error: Failed to collect walltime results. This may be due to version incompatibility. Ensure that your compat layer (codspeed-criterion-compat, codspeed-bencher-compat, or codspeed-divan-compat) has the same major version as cargo-codspeed (currently v3.0.2).
```

Retrying failed a few times in a row. Gonna see if rebasing helps :)

Edit: That worked. #codemonkeywritecode

---

_Merged by @oconnor663 on 2025-07-01 16:52_

---

_Closed by @oconnor663 on 2025-07-01 16:52_

---

_Branch deleted on 2025-07-01 16:52_

---

_Comment by @oconnor663 on 2025-07-03 22:12_

https://github.com/astral-sh/uv/actions/runs/16060816582/job/45326189810 includes this change and does _not_ indicate that it retried. Does that make it the smoking gun for https://github.com/astral-sh/uv/issues/14425 ?

---
