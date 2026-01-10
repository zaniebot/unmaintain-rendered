```yaml
number: 9267
title: Unify cargo features
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cargo-features
created_at: 2024-11-20T09:30:20Z
updated_at: 2024-11-20T15:11:26Z
url: https://github.com/astral-sh/uv/pull/9267
synced_at: 2026-01-10T12:00:00Z
```

# Unify cargo features

---

_Pull request opened by @konstin on 2024-11-20 09:30_

When building only a single crate in the workspace to run its tests, we often recompile a lot of other, unrelated crates. Whenever cargo has a different set of crate features, it needs to recompile. By moving some features (non-exhaustive for now) to the workspace level, we always activate them an avoid recompiling.

The cargo docs mismatch the behavior of cargo around default-deps, so I filed that upstream and left most `default-features` mismatches: https://github.com/rust-lang/cargo/issues/14841.

Reference script:

```python
import tomllib
from collections import defaultdict
from pathlib import Path

uv = Path("/home/konsti/projects/uv")
skip_list = ["uv-trampoline", "uv-dev", "uv-performance-flate2-backend", "uv-performance-memory-allocator"]

root_feature_map = defaultdict(set)
root_default_features = defaultdict(bool)
cargo_toml = tomllib.loads(uv.joinpath("Cargo.toml").read_text())
for dep, declaration in cargo_toml["workspace"]["dependencies"].items():
    root_default_features[dep] = root_default_features[dep] or declaration.get("default-features", True)
    root_feature_map[dep].update(declaration.get("features", []))

feature_map = defaultdict(set)
default_features = defaultdict(bool)
for crate in uv.joinpath("crates").iterdir():
    if crate.name in skip_list:
        continue
    if not crate.joinpath("Cargo.toml").is_file():
        continue
    cargo_toml = tomllib.loads(crate.joinpath("Cargo.toml").read_text())
    for dep, declaration in cargo_toml.get("dependencies", {}).items():
        # If any item uses default features, they are used everywhere
        default_features[dep] = default_features[dep] or declaration.get("default-features", True)
        feature_map[dep].update(declaration.get("features", []))

for dep, features in sorted(feature_map.items()):
    features = features - root_feature_map.get(dep, set())
    if not features and default_features[dep] == root_default_features[dep]:
        continue
    print(dep, default_features[dep], sorted(features))
```

---

_Label `internal` added by @konstin on 2024-11-20 09:30_

---

_@charliermarsh approved on 2024-11-20 14:06_

---

_@zanieb approved on 2024-11-20 14:28_

---

_Merged by @konstin on 2024-11-20 15:11_

---

_Closed by @konstin on 2024-11-20 15:11_

---

_Branch deleted on 2024-11-20 15:11_

---
