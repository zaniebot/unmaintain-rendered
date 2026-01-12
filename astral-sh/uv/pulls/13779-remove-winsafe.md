```yaml
number: 13779
title: Remove winsafe
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/remove-winsafe
created_at: 2025-06-02T07:18:19Z
updated_at: 2025-06-02T16:22:52Z
url: https://github.com/astral-sh/uv/pull/13779
synced_at: 2026-01-12T16:10:51Z
```

# Remove winsafe

---

_@konstin_

We've been using a number of different winapi crates. This PR removes winsafe in favor of the official windows-* crates, so all of uv's own winapi calls go through the official windows-* crates.

---

_Review requested from @Gankra by @konstin on 2025-06-02 07:18_

---

_Label `windows` added by @konstin on 2025-06-02 07:18_

---

_Label `enhancement` added by @konstin on 2025-06-02 07:18_

---

_Review comment by @Gankra on `crates/uv-fs/Cargo.toml`:34 on 2025-06-02 14:34_

refactor cruft?

---

_Review comment by @Gankra on `crates/uv-fs/src/which.rs`:3 on 2025-06-02 14:35_

```suggestion
#[cfg(windows)]
```

(Either is fine, we use this one more)

---

_@Gankra approved on 2025-06-02 14:36_

Seems reasonable -- is this resolving a problem?

---

_@konstin reviewed on 2025-06-02 14:42_

---

_Review comment by @konstin on `crates/uv-fs/Cargo.toml`:34 on 2025-06-02 14:42_

oops thx

---

_Comment by @konstin on 2025-06-02 14:46_

> Seems reasonable -- is this resolving a problem?

We had a number of different winapi crates all with different conventions, sometimes for the same job in different parts of the code. I've removed winreg before (#11805), this PR should move uv to only use the official windows-* crates.

---

_Merged by @konstin on 2025-06-02 15:53_

---

_Closed by @konstin on 2025-06-02 15:53_

---

_Branch deleted on 2025-06-02 15:53_

---

_Comment by @konstin on 2025-06-02 15:59_

(sorry, i discovered when writing the team message for the merge)

@BurntSushi Could this unsafe be removed through https://docs.rs/winapi-util/latest/winapi_util/file/fn.typ.html? I just saw that we have winapi-util in our dep tree already.

---

_Comment by @BurntSushi on 2025-06-02 16:22_

@konstin I don't think so. [`GetBinaryType`](https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-getbinarytypea) and [`GetFileType`](https://learn.microsoft.com/en-us/windows/win32/api/fileapi/nf-fileapi-getfiletype) seem like different routines. The former is about executable formats while the latter is the more traditional "is a file or is a directory" test. The error conditions are also likely different.

---
