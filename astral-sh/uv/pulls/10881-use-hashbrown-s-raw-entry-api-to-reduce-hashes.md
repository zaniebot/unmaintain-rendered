```yaml
number: 10881
title: "Use Hashbrown's raw entry API to reduce hashes and clone in priority"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/raw
created_at: 2025-01-23T02:40:35Z
updated_at: 2025-01-23T14:34:40Z
url: https://github.com/astral-sh/uv/pull/10881
synced_at: 2026-01-10T11:45:16Z
```

# Use Hashbrown's raw entry API to reduce hashes and clone in priority

---

_Pull request opened by @charliermarsh on 2025-01-23 02:40_

## Summary

I'm open to not merging this -- I was kind of just interested in what the API looked like. But the idea is: we can avoid hashing values twice and unnecessarily cloning within the priority map by using the raw entry API.


---

_Review requested from @konstin by @charliermarsh on 2025-01-23 02:40_

---

_Label `performance` added by @charliermarsh on 2025-01-23 02:40_

---

_@zanieb approved on 2025-01-23 04:57_

I don't find this hard to read

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:58 on 2025-01-23 09:54_

What about using the `EntryRef` API?

```rust
        let len = self.virtual_package_tiebreaker.len();
        self.virtual_package_tiebreaker
            .entry_ref(package)
            .or_insert_with(|| {
                PubGrubTiebreaker::from(u32::try_from(len).expect("Less than 2**32 packages"))
            });
```

---

_@konstin reviewed on 2025-01-23 09:54_

---

_@charliermarsh reviewed on 2025-01-23 13:10_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:58 on 2025-01-23 13:10_

That doesn’t work unless PubGrubPackage implements From<&PubGrubPackage> for PubGrubPackage.

---

_@konstin reviewed on 2025-01-23 13:28_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:58 on 2025-01-23 13:28_

A bit annoying that they use `From` instead of `ToOwned`, but treating it as clone works 

> The relationships I use between the key `K` and borrowed key `Q` is `K: Borrow<Q> + From<&Q>`. The reason for not using `Q: ToOwned<K>` is to support key types like `Rc<str>` (which is used in a couple of the doctests), which would not be possible with `to_owned()`.

(from https://github.com/rust-lang/hashbrown/pull/301)

```rust
impl From<&PubGrubPackage> for PubGrubPackage {
    fn from(package: &PubGrubPackage) -> Self {
        package.clone()
    }
}
```

---

_@charliermarsh reviewed on 2025-01-23 13:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:58 on 2025-01-23 13:56_

Ok, we can do that instead.

---

_@konstin approved on 2025-01-23 14:32_

---

_Merged by @charliermarsh on 2025-01-23 14:34_

---

_Closed by @charliermarsh on 2025-01-23 14:34_

---

_Branch deleted on 2025-01-23 14:34_

---
