```yaml
number: 3698
title: Add PEP 714 support for JSON API client
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/dist-info-json
created_at: 2024-05-21T14:13:10Z
updated_at: 2024-05-21T15:52:39Z
url: https://github.com/astral-sh/uv/pull/3698
synced_at: 2026-01-10T14:32:20Z
```

# Add PEP 714 support for JSON API client

---

_Pull request opened by @charliermarsh on 2024-05-21 14:13_

## Summary

Closes https://github.com/astral-sh/uv/issues/3689.

## Test Plan

Manually verified we pick up `core-metadata` from PyPI if I remove the aliases.


---

_Label `compatibility` added by @charliermarsh on 2024-05-21 14:13_

---

_Comment by @charliermarsh on 2024-05-21 14:44_

Oh interesting, Serde does not treat aliases as fallbacks. They're mutually exclusive.

---

_Review requested from @zanieb by @charliermarsh on 2024-05-21 14:51_

---

_Review requested from @konstin by @charliermarsh on 2024-05-21 14:51_

---

_@charliermarsh reviewed on 2024-05-21 14:52_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 14:52_

Repeating like this is the only way I could figure out to support multiple aliases under different names, where _multiple_ values can be present. (PyPI serves both `core-metadata` and `data-dist-info-metadata`; serde's ` alias` feature errors if multiple values are present.)

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 14:52_

Per PEP 714, we _could_ omit `data_dist_info_metadata`... But there may be registries that _only_ serve that in practice, since that's what pip did for a while.

---

_@charliermarsh reviewed on 2024-05-21 14:52_

---

_@BurntSushi reviewed on 2024-05-21 15:10_

---

_Review comment by @BurntSushi on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 15:10_

I would define a separate wire type with what you want to support, and then implement `From<FileWire> for File` (or `TryFrom` if needed). Then use [`#[serde(from = "FileWire")]`](https://serde.rs/container-attrs.html#from). That way, you can resolve the aliases down to one field at deserialization time, and avoid spreading that complexity outward to anyone consuming `File`.

---

_@charliermarsh reviewed on 2024-05-21 15:11_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 15:11_

Hmm, but we already have a separate `File` type that we use in our own abstractions.

---

_@charliermarsh reviewed on 2024-05-21 15:11_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 15:11_

So in a sense this _is_ `FileWire`. And we have a `From` elsewhere.

---

_@BurntSushi reviewed on 2024-05-21 15:14_

---

_Review comment by @BurntSushi on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 15:14_

Ah I was confused because I wouldn't normally expect a wire type to have `pub` fields and otherwise be exported. Basically, my suggestion boils down to "encapsulate the aliases at the deserialization boundary." But if the point of `File` is to be the boundary and be exposed to the world, then yeah, I think what you have is fine.

---

_@charliermarsh reviewed on 2024-05-21 15:21_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/simple_json.rs`:44 on 2024-05-21 15:21_

Yeah this is meant to be roughly "the exact JSON we get from the API", so I suppose this is ok.

---

_Merged by @charliermarsh on 2024-05-21 15:52_

---

_Closed by @charliermarsh on 2024-05-21 15:52_

---

_Branch deleted on 2024-05-21 15:52_

---
