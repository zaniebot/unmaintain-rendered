```yaml
number: 3651
title: "Use filename trait for `WheelWire` conversion"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/decode
created_at: 2024-05-18T17:07:40Z
updated_at: 2024-05-20T13:25:32Z
url: https://github.com/astral-sh/uv/pull/3651
synced_at: 2026-01-10T14:32:20Z
```

# Use filename trait for `WheelWire` conversion

---

_Pull request opened by @charliermarsh on 2024-05-18 17:07_

## Summary

The main motivation here is that the `.filename()` method that we implement on `Url` will do URL decoding for the last segment, which we were missing here.

The errors are a bit awkward, because in `crates/uv-resolver/src/lock.rs`, we wrap in `failed to extract filename from URL: {url}`, so in theory we want the underlying errors to _omit_ the URL? But sometimes they use `#[error(transparent)]`?


---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-18 17:07_

---

_Label `bug` added by @charliermarsh on 2024-05-18 17:07_

---

_@charliermarsh reviewed on 2024-05-18 17:08_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:710 on 2024-05-18 17:08_

Ported this over from your version in `lock.rs`.

---

_@charliermarsh reviewed on 2024-05-18 17:09_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/error.rs`:20 on 2024-05-18 17:09_

Weird that this omits the URL, but `MissingFilePath` includes it, because the clients of these errors sometimes include the URL and sometimes don't. Specifically, where we return `MissingFilePath`, we need to include the URL; but where we return `MissingPathSegments`, we want to exclude it, because we wrap it in a message that includes the URL (so including it here would duplicate it). I don't know what the right strategy is.

---

_@charliermarsh reviewed on 2024-05-18 17:11_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/error.rs`:20 on 2024-05-18 17:11_

Okay, changed these to _both_ include the URL, and removed the "wrap the error in `failed to extract filename from URL`" behavior from `lock.rs`.

---

_@BurntSushi approved on 2024-05-20 12:28_

Makes sense to me!

---

_Merged by @charliermarsh on 2024-05-20 13:25_

---

_Closed by @charliermarsh on 2024-05-20 13:25_

---

_Branch deleted on 2024-05-20 13:25_

---
