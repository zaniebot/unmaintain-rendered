```yaml
number: 4407
title: Support toolchain requests with platform-tag style Python implementations and version
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/request-platform-tag
created_at: 2024-06-18T23:54:27Z
updated_at: 2024-06-19T17:04:25Z
url: https://github.com/astral-sh/uv/pull/4407
synced_at: 2026-01-12T16:06:12Z
```

# Support toolchain requests with platform-tag style Python implementations and version

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4399

---

_Label `enhancement` added by @zanieb on 2024-06-18 23:54_

---

_Comment by @zanieb on 2024-06-18 23:54_

cc @fynnsu

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:1299 on 2024-06-18 23:55_

@BurntSushi I'm embarrassed :)

---

_@zanieb reviewed on 2024-06-18 23:55_

---

_Review requested from @konstin by @zanieb on 2024-06-18 23:56_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-18 23:56_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:1279 on 2024-06-19 13:40_

It took me a while to parse this. (With a huge expression on the right hand side of an `if let`.) I'd probably move it up and give it a name?

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:1280 on 2024-06-19 13:43_

Hmmm this looks odd to me. I suppose it makes the use of pattern matching nicer below, but I'd probably just work in ASCII land here:

```rust
let bytes = s.as_bytes();
if bytes.len() == 1 {
    if (b'0'..=b'9').contains(&bytes[0]) {
        Some(VersionRequest::Major(bytes[0] - b'0'))
    } else {
        None
    }
}
```

And so on for the `MajorMinor`. If you split this into a helper function, you could probably make it even simpler via early returns.

---

_@BurntSushi approved on 2024-06-19 13:44_

Nice! I think I would probably write some of this code differently, but I defer to you. :)

---

_Merged by @zanieb on 2024-06-19 17:04_

---

_Closed by @zanieb on 2024-06-19 17:04_

---

_Branch deleted on 2024-06-19 17:04_

---
