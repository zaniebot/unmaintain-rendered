```yaml
number: 1082
title: Use a buffered reader for wheel metadata
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/buf
created_at: 2024-01-24T20:09:12Z
updated_at: 2024-01-25T13:48:32Z
url: https://github.com/astral-sh/uv/pull/1082
synced_at: 2026-01-12T16:04:24Z
```

# Use a buffered reader for wheel metadata

---

_@charliermarsh_

## Summary

It turns out this is significantly faster when reading (e.g.) _just_ the `METADATA` file from a zipped wheel.

I audited other `File::open` usages, and everything else seems to be using a buffered reader already (directly, or in whatever third-party crate it's passed to) _or_ is read immediately in full.

See the criterion benchmark:

```
file_reader/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [6.9618 ms 6.9664 ms 6.9713 ms]
Found 4 outliers among 100 measurements (4.00%)
  4 (4.00%) high mild
file_reader/flask-3.0.1-py3-none-any.whl
                        time:   [237.50 µs 238.25 µs 239.13 µs]
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe

buffered_reader/numpy-1.26.3-pp39-pypy39_pp73-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
                        time:   [648.92 µs 653.85 µs 660.09 µs]
Found 4 outliers among 100 measurements (4.00%)
  3 (3.00%) high mild
  1 (1.00%) high severe
buffered_reader/flask-3.0.1-py3-none-any.whl
                        time:   [39.578 µs 39.712 µs 39.869 µs]
Found 8 outliers among 100 measurements (8.00%)
  3 (3.00%) high mild
  5 (5.00%) high severe
```

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-24 20:09_

---

_@charliermarsh reviewed on 2024-01-24 20:09_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/download.rs`:139 on 2024-01-24 20:09_

It turns out this was unused. Sigh! Need multi-crate detection!

---

_Comment by @konstin on 2024-01-24 20:18_

Astounding! I checked that the sync zip crates used buffered reader internally, and async zip [does too](https://github.com/Majored/rs-async-zip/blob/7ae5331e5de730cb87e02f6647bcaba278a8ddbf/src/base/read/seek.rs#L100), i'm surprised by this massive speedup

---

_@konstin approved on 2024-01-24 20:18_

---

_Comment by @charliermarsh on 2024-01-24 20:22_

It could just be an artifact of the benchmark, I guess, but it seems real?

---

_Merged by @charliermarsh on 2024-01-24 20:22_

---

_Closed by @charliermarsh on 2024-01-24 20:22_

---

_Branch deleted on 2024-01-24 20:22_

---

_Review comment by @BurntSushi on `crates/puffin-client/src/registry_client.rs`:249 on 2024-01-25 13:21_

The interesting thing here is that it seems like `async_zip` does eventually create a buffered reader: https://github.com/Majored/rs-async-zip/blob/7ae5331e5de730cb87e02f6647bcaba278a8ddbf/src/base/read/mod.rs#L83-L87

But it does do (by my count) several reads before that.

¯\\\_(ツ)_/¯

---

_@BurntSushi reviewed on 2024-01-25 13:21_

---

_@charliermarsh reviewed on 2024-01-25 13:48_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:249 on 2024-01-25 13:48_

The common case for reading metadata actually goes through the synchronous reader, not here. My benchmarks are for the synchronous reader. So I actually don’t know if it makes an impact here.

---
