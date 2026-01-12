```yaml
number: 1546
title: Avoid attempting rename in copy fallback path
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-02-16T22:02:45Z
updated_at: 2024-02-16T23:28:12Z
url: https://github.com/astral-sh/uv/pull/1546
synced_at: 2026-01-12T16:04:39Z
```

# Avoid attempting rename in copy fallback path

---

_@charliermarsh_

## Summary

This _could_ fix https://github.com/astral-sh/uv/issues/1454, but I'm not sure. I was able to replicate by forcing a bunch of error states. But, in short, if we fail to hardlink on the initial copy due to a file existing, and then fail _again_, we fallback to copying. But if we copy, then the tempfile doesn't exist, and so the `fs_err::rename(&tempfile, &out_path)?;` will fail with "File not found".

This PR just ensures that the cases are explicitly mutually exclusive: we only attempt to rename if the hardlink succeeded.

---

_Label `bug` added by @charliermarsh on 2024-02-16 22:03_

---

_Merged by @charliermarsh on 2024-02-16 22:08_

---

_Closed by @charliermarsh on 2024-02-16 22:08_

---

_Branch deleted on 2024-02-16 22:08_

---

_@T-256 reviewed on 2024-02-16 23:12_

---

_Review comment by @T-256 on `crates/install-wheel-rs/src/linker.rs`:423 on 2024-02-16 23:12_

Is this really changed behavior than it was before?
IMO you could handle `fs_err::rename` error instead:

```rs
                        if fs::hard_link(path, &tempfile).is_err() {
                            fs::copy(path, &out_path)?;
                            attempt = Attempt::UseCopyFallback;
                        }
                        if fs_err::rename(&tempfile, &out_path).is_err() {
                            fs::copy(path, &out_path)?;
                            attempt = Attempt::UseCopyFallback;
                        }
```

---

_@charliermarsh reviewed on 2024-02-16 23:17_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:423 on 2024-02-16 23:17_

Yeah, it's a change, right? Before, we did `fs_err::rename(&tempfile, &out_path)?;` unequivocally. Now, we only do it in the event that the hard link fails.

---

_@T-256 reviewed on 2024-02-16 23:28_

---

_Review comment by @T-256 on `crates/install-wheel-rs/src/linker.rs`:423 on 2024-02-16 23:28_


> we did `fs_err::rename(&tempfile, &out_path)?;` unequivocally

Sorry, my bad.
I thought it will send return from if-block (at python we do mostly if-return instead of if-else 😄).



---
