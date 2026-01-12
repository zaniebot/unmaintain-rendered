```yaml
number: 13625
title: Windows paths are valid URLs
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: konsti/windows-paths-are-valid-urls
created_at: 2025-05-23T19:01:57Z
updated_at: 2025-05-28T15:11:58Z
url: https://github.com/astral-sh/uv/pull/13625
synced_at: 2026-01-12T16:10:46Z
```

# Windows paths are valid URLs

---

_@konstin_

Currently, it is not possible to set a custom Python downloads JSON on Windows, as Windows paths can be valid URLs.

```rust
use url::Url;

fn main() {
    dbg!(Url::parse(r"C:\Users\Ferris\download.json"));
}
```

Tested by https://github.com/astral-sh/uv/pull/13585 (where it is currently failing CI).

---

_Review requested from @Gankra by @konstin on 2025-05-23 19:01_

---

_Label `bug` added by @konstin on 2025-05-23 19:01_

---

_Label `windows` added by @konstin on 2025-05-23 19:01_

---

_@charliermarsh approved on 2025-05-23 19:04_

---

_Comment by @charliermarsh on 2025-05-23 19:05_

I might suggest using `split_scheme` and `Scheme::parse` here instead of `Url::parse`? That's what we tend to do elsewhere.

---

_Comment by @konstin on 2025-05-23 19:07_

To support `file:` URLs or to check something more specific than `Url::parse`?

---

_Comment by @charliermarsh on 2025-05-23 19:07_

To check something more specific, e.g., that it's a known remote host like http or https.

---

_Comment by @charliermarsh on 2025-05-24 15:06_

As-is, won't this erroneously tell you that remote URLs aren't supported if you use a Windows path and it fails to open?

---

_Comment by @konstin on 2025-05-26 10:49_

Oh now I understand, good catch.

I've changed the logic to assume that one-letter protocols are Windows drives, and to also support `file://` URLs while I'm at it.

---

_Comment by @charliermarsh on 2025-05-26 11:19_

Why not just use the scheme detection code we use everywhere else?

---

_Comment by @Gankra on 2025-05-27 03:25_

I agree with charlie, we should be allow-listing http: and https: instead of trying to detect file paths in particular.

---

_Comment by @konstin on 2025-05-27 14:33_

wdym with allow-listing http(s)? I'm specifically only fixing the Windows case that turned out to be broken in #13585, without adding support for remote URLs.

---

_Comment by @charliermarsh on 2025-05-27 14:58_

I think the suggestion is that this error should only show up if the scheme is `http` or `https`.

---

_Comment by @konstin on 2025-05-27 14:58_

Make sense!

---

_@zanieb reviewed on 2025-05-28 14:11_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:96 on 2025-05-28 14:11_

Not your change, but "python" should be capitalized.

---

_@zanieb approved on 2025-05-28 14:11_

---

_Merged by @zanieb on 2025-05-28 15:11_

---

_Closed by @zanieb on 2025-05-28 15:11_

---

_Branch deleted on 2025-05-28 15:11_

---
