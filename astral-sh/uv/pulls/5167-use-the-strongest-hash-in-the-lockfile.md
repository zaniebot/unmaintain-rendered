```yaml
number: 5167
title: Use the strongest hash in the lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/best-hash
created_at: 2024-07-17T20:12:56Z
updated_at: 2024-07-17T20:38:34Z
url: https://github.com/astral-sh/uv/pull/5167
synced_at: 2026-01-12T16:06:40Z
```

# Use the strongest hash in the lockfile

---

_@charliermarsh_

## Summary

We only need to store one hash -- it should be the "strongest" hash. In practice, most registries (like PyPI) only serve one, and we only compute a SHA256 hash for direct URLs.

Part of: https://github.com/astral-sh/uv/issues/4924

## Test Plan

I verified that changing:

```diff
diff --git a/crates/distribution-types/src/hash.rs b/crates/distribution-types/src/hash.rs
index 553a74f55..d36c62286 100644
--- a/crates/distribution-types/src/hash.rs
+++ b/crates/distribution-types/src/hash.rs
@@ -31,7 +31,7 @@ impl<'a> HashPolicy<'a> {
     pub fn algorithms(&self) -> Vec<HashAlgorithm> {
         match self {
             Self::None => vec![],
-            Self::Generate => vec![HashAlgorithm::Sha256],
+            Self::Generate => vec![HashAlgorithm::Sha256, HashAlgorithm::Sha512],
             Self::Validate(hashes) => {
                 let mut algorithms = hashes.iter().map(HashDigest::algorithm).collect::<Vec<_>>();
                 algorithms.sort();
```

Then running `uv lock` with a URL gave me:

```toml
[[distribution]]
name = "iniconfig"
version = "2.0.0"
source = { url = "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl", hash = "sha512:44cc53a6c8dd7cf4d6d52bded308bcc4b4f85fff2ed081f60f7d4beaa86a7cde6d099e3976331232d4cbd472ad5d1781064725b0999c7cd3a2a4d42df687ee81" },
]
```


---

_Label `preview` added by @charliermarsh on 2024-07-17 20:13_

---

_Marked ready for review by @charliermarsh on 2024-07-17 20:13_

---

_Merged by @charliermarsh on 2024-07-17 20:38_

---

_Closed by @charliermarsh on 2024-07-17 20:38_

---

_Branch deleted on 2024-07-17 20:38_

---
