```yaml
number: 5888
title: Enforce extension validity at parse time
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2024-08-07T19:37:31Z
updated_at: 2024-08-09T01:39:49Z
url: https://github.com/astral-sh/uv/pull/5888
synced_at: 2026-01-10T13:31:54Z
```

# Enforce extension validity at parse time

---

_Pull request opened by @charliermarsh on 2024-08-07 19:37_

## Summary

This PR adds a `DistExtension` field to some of our distribution types, which requires that we validate that the file type is known and supported when parsing (rather than when attempting to unzip). It removes a bunch of extension parsing from the code too, in favor of doing it once upfront.

Closes https://github.com/astral-sh/uv/issues/5858.


---

_Label `internal` added by @charliermarsh on 2024-08-07 19:37_

---

_Comment by @charliermarsh on 2024-08-07 19:37_

I think this is a good change but it's more aggressive. It means we might error if a registry serves distribution kinds that we don't support, even if we'd never read them.

---

_Comment by @charliermarsh on 2024-08-07 19:39_

Oh nevermind, we wouldn't fail in that case due to some other gating.

---

_@zanieb reviewed on 2024-08-07 20:24_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:5714 on 2024-08-07 20:24_

Can we say "file extension"? Why did this succeed previously?

---

_@zanieb approved on 2024-08-07 20:25_

Basically down for this behavior.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-07 21:54_

---

_Review requested from @konstin by @charliermarsh on 2024-08-07 21:54_

---

_@charliermarsh reviewed on 2024-08-07 21:55_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/extension.rs`:26 on 2024-08-07 21:55_

The existing definition was:
```rust
#[derive(
    Clone,
    Debug,
    PartialEq,
    Eq,
    Serialize,
    Deserialize,
    rkyv::Archive,
    rkyv::Deserialize,
    rkyv::Serialize,
)]
#[archive(check_bytes)]
#[archive_attr(derive(Debug))]
pub enum SourceDistExtension {
    Zip,
    TarGz,
    TarBz2,
}
```

I think this is safe without a cache bump? @BurntSushi?

---

_Marked ready for review by @charliermarsh on 2024-08-07 22:26_

---

_Review comment by @konstin on `crates/distribution-filename/src/extension.rs`:88 on 2024-08-08 07:56_

Below we serialize this as `zstd`

---

_@konstin approved on 2024-08-08 08:03_

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/extension.rs`:19 on 2024-08-08 11:46_

I tried to find where rkyv was used with `DistExtension`, but couldn't. Do we need rkyv support for `DistExtension`? If not, I'd drop the trait impls and `archive` annotations below.

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/extension.rs`:26 on 2024-08-08 11:50_

Hmmmmmmmmmm. Likely fine, but I actually do not believe it is guaranteed. rkyv doesn't have great docs on what kinds of changes are compatible with existing serialized data. I would generally assume that _any_ change requires a cache bump, even if in some cases, it isn't _in practice_ required.

---

_Review comment by @BurntSushi on `crates/distribution-filename/src/extension.rs`:83 on 2024-08-08 11:51_

There might be an opportunity for a helper function here or an extra local variable. The same chunk of path extension code is repeated for each case here (and `zst` below).

---

_@BurntSushi approved on 2024-08-08 11:54_

Yes, I like this!

---

_Comment by @charliermarsh on 2024-08-08 17:06_

I bumped the rkyv cache, to be conservative.

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/extension.rs`:88 on 2024-08-08 17:06_

Thanks, that's wrong!

---

_@charliermarsh reviewed on 2024-08-08 17:06_

---

_Merged by @charliermarsh on 2024-08-09 01:39_

---

_Closed by @charliermarsh on 2024-08-09 01:39_

---

_Branch deleted on 2024-08-09 01:39_

---
