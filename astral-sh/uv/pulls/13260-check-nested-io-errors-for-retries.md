```yaml
number: 13260
title: Check nested IO errors for retries
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - network
assignees: []
merged: true
base: main
head: konsti/check-nested-io-errors
created_at: 2025-05-02T09:08:59Z
updated_at: 2025-05-02T12:41:11Z
url: https://github.com/astral-sh/uv/pull/13260
synced_at: 2026-01-10T11:10:41Z
```

# Check nested IO errors for retries

---

_Pull request opened by @konstin on 2025-05-02 09:08_

## Summary

The only thing that changed for #12175 relevant to the existing downloads is the order of nesting, so we're checking all nested IO errors instead of only the first one.

See #13238

## Test Plan

This is an educated guess based on what happens if I turn off the network during a download.

```
Downloading cpython-3.13.3-linux-x86_64-gnu (download) (20.3MiB)
TRACE Considering retry of error: ExtractError("cpython-3.13.3-20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz", Io(Custom { kind: Other, error: TarError { desc: "failed to unpack `/home/konsti/.local/share/uv/python/.temp/.tmpe3AIvt/python/lib/libpython3.13.so.1.0`", io: Custom { kind: Other, error: TarError { desc: "failed to unpack `python/lib/libpython3.13.so.1.0` into `/home/konsti/.local/share/uv/python/.temp/.tmpe3AIvt/python/lib/libpython3.13.so.1.0`", io: Custom { kind: Other, error: reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: TimedOut } } } } } } }))
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry error: not an IO error
error: Failed to install cpython-3.13.3-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.13.3-20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: failed to unpack `/home/konsti/.local/share/uv/python/.temp/.tmpe3AIvt/python/lib/libpython3.13.so.1.0`
  Caused by: failed to unpack `python/lib/libpython3.13.so.1.0` into `/home/konsti/.local/share/uv/python/.temp/.tmpe3AIvt/python/lib/libpython3.13.so.1.0`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: operation timed out
``` 

---

_Label `enhancement` added by @konstin on 2025-05-02 09:08_

---

_Label `network` added by @konstin on 2025-05-02 09:08_

---

_Review requested from @Gankra by @konstin on 2025-05-02 09:08_

---

_@zanieb approved on 2025-05-02 12:21_

Interesting, thank you!

---

_Merged by @konstin on 2025-05-02 12:41_

---

_Closed by @konstin on 2025-05-02 12:41_

---

_Branch deleted on 2025-05-02 12:41_

---
