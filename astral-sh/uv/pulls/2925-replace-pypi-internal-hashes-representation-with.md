```yaml
number: 2925
title: Replace PyPI-internal Hashes representation with flat vector
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/digest
created_at: 2024-04-09T03:00:00Z
updated_at: 2024-04-09T16:56:17Z
url: https://github.com/astral-sh/uv/pull/2925
synced_at: 2026-01-10T14:43:31Z
```

# Replace PyPI-internal Hashes representation with flat vector

---

_Pull request opened by @charliermarsh on 2024-04-09 03:00_

## Summary

Right now, we have a `Hashes` representation that looks like:

```rust
/// A dictionary mapping a hash name to a hex encoded digest of the file.
///
/// PEP 691 says multiple hashes can be included and the interpretation is left to the client.
#[derive(Debug, Clone, Eq, PartialEq, Default, Deserialize)]
pub struct Hashes {
    pub md5: Option<Box<str>>,
    pub sha256: Option<Box<str>>,
    pub sha384: Option<Box<str>>,
    pub sha512: Option<Box<str>>,
}
```

It stems from the PyPI API, which returns a dictionary of hashes.

We tend to pass these around as a vector of `Vec<Hashes>`. But it's a bit strange because each entry in that vector could contain multiple hashes. And it makes it difficult to ask questions like "Is `sha256:ab21378ca980a8` in the set of hashes"?

This PR instead treats `Hashes` as the PyPI-internal type, and uses a new `Vec<HashDigest>` everywhere in our own APIs.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-09 03:16_

---

_Label `internal` added by @charliermarsh on 2024-04-09 03:16_

---

_Marked ready for review by @charliermarsh on 2024-04-09 03:16_

---

_@charliermarsh reviewed on 2024-04-09 03:16_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:28 on 2024-04-09 03:16_

I also changed this because it relies on `Hashes` internally.

---

_@charliermarsh reviewed on 2024-04-09 03:17_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:293 on 2024-04-09 03:17_

@BurntSushi - Do you agree with this TODO? I'm sort of undecided on it.

---

_@konstin approved on 2024-04-09 08:43_

---

_Review comment by @BurntSushi on `crates/uv-cache/src/lib.rs`:601 on 2024-04-09 12:38_

Nice, I was looking for this. :-)

---

_Review comment by @BurntSushi on `crates/pypi-types/src/simple_json.rs`:293 on 2024-04-09 12:41_

Hmmm. Say more? This is a digest right? A digest is a human readable alphanumeric representation of the actual hash value. So if that's what you're storing, then making it a string seems sensible to me. But you could instead store the hash value itself, in which case it would _have_ to be a `Vec<u8>` (or perhaps a `Box<[u8]>` in this case). Whether you want a digest or the hash value itself isn't totally clear to me, but it may not matter.

---

_Review comment by @BurntSushi on `crates/pypi-types/src/simple_json.rs`:141 on 2024-04-09 13:03_

Does inserting this first cause a change in behavior? It looks like `as_str()` previously preferred `sha512` for example, but I think I saw some calls to `hashes.first()` above which would prefer `md5` I think?

---

_@BurntSushi approved on 2024-04-09 13:06_

Seems sensible to me!

There are perhaps other possible abstractions to use here. For example, as long as there is at most one digest per hash type, you could use a map:

```rust
enum HashAlgorithm { Md5 = 0, Sha256 = 1, Sha384 = 2, Sha512 = 3 }

struct HashDigests {
    map: [Option<Box<str>>; 4],
}
```

Then asking whether a particular hash digests is something like `digests.map[HashAlgorithm::Md5 as usize].is_some()`.

But, I like your representation better because it seems simpler to me.

One final thought:

> This PR instead treats Hashes as the PyPI-internal type, and uses a new `Vec<HashDigest>` everywhere in our own APIs.

if instead we defined a new `HashDigests` type that encapsulates the representation and exposes just the ops you need, then it'd be easier to change the representation in the future. And _maybe_ make some caller code simpler. But I don't have any real issue with just using `Vec<HashDigest>`.

---

_@charliermarsh reviewed on 2024-04-09 15:22_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:141 on 2024-04-09 15:22_

Hmm, I guess it _could_ yes. I'll change it to put the highest-security hashes first.

---

_@charliermarsh reviewed on 2024-04-09 15:22_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:293 on 2024-04-09 15:22_

Yeah it's a digest. I guess using the human-readable value is nice.

---

_Comment by @charliermarsh on 2024-04-09 16:16_

Yeah the benefit of the current `Hashes` representation is that it automatically deserializes from the JSON. Ultimately, we'll have a set of hashes for which there could be multiple digests with the same algorithm, each corresponding to a different distribution, so we do need some kind of `HashSet<HashDigest>` rather than a fixed list.

---

_Merged by @charliermarsh on 2024-04-09 16:56_

---

_Closed by @charliermarsh on 2024-04-09 16:56_

---

_Branch deleted on 2024-04-09 16:56_

---
