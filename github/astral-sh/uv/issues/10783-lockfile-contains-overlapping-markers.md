---
number: 10783
title: Lockfile contains overlapping markers
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-01-20T17:22:44Z
updated_at: 2025-01-21T19:04:32Z
url: https://github.com/astral-sh/uv/issues/10783
synced_at: 2026-01-07T13:12:18-06:00
---

# Lockfile contains overlapping markers

---

_Issue opened by @charliermarsh on 2025-01-20 17:22_

Given:

```toml
[project]
name = "ads-mega-model"
version = "0.1.0"
requires-python = "==3.10.12"
dependencies = [
    "wandb==0.17.6",
]

[project.optional-dependencies]
cpu = [
  "torch==2.2.2",
]
cu118 = [
  "torch==2.2.2",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu118" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

The `uv.lock` contains:

```toml
resolution-markers = [
    "sys_platform == 'linux'",
    "sys_platform != 'linux'",
    "platform_machine != 'aarch64' and sys_platform == 'linux'",
    "sys_platform != 'darwin' and sys_platform != 'linux'",
    "platform_machine == 'aarch64' and sys_platform == 'linux'",
    "sys_platform == 'darwin'",
]
```

See: https://github.com/astral-sh/uv/issues/10772#issuecomment-2602682548


---

_Label `bug` added by @charliermarsh on 2025-01-20 17:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-20 17:22_

---

_Referenced in [astral-sh/uv#10772](../../astral-sh/uv/issues/10772.md) on 2025-01-20 17:23_

---

_Comment by @charliermarsh on 2025-01-20 17:27_

I think this is a conflicting extras problem, @BurntSushi?

The universal markers at the end of the resolution are:

```
python_full_version >= '3.10' and sys_platform == 'linux' and extra != 'extra-14-ads-mega-model-cpu' and extra == 'extra-14-ads-mega-model-cu118'
sys_platform != 'linux' and extra != 'extra-14-ads-mega-model-cpu' and extra == 'extra-14-ads-mega-model-cu118'
python_full_version >= '3.10' and platform_machine != 'aarch64' and sys_platform == 'linux' and extra == 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
sys_platform != 'darwin' and sys_platform != 'linux' and extra == 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
platform_machine == 'aarch64' and sys_platform == 'linux' and extra == 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
sys_platform == 'darwin' and extra == 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
python_full_version >= '3.10' and sys_platform == 'linux' and extra != 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
sys_platform != 'linux' and extra != 'extra-14-ads-mega-model-cpu' and extra != 'extra-14-ads-mega-model-cu118'
```

---

_Comment by @charliermarsh on 2025-01-20 17:30_

I guess it's this?

```rust
/// Returns the simplified string-ified version of each marker given.
///
/// If a marker is a duplicate of a previous marker or is always true after
/// simplification, then it is omitted from the `Vec` returned. (And indeed,
/// the `Vec` returned may be empty.)
fn deduplicated_simplified_pep508_markers(
    markers: &[UniversalMarker],
    requires_python: &RequiresPython,
) -> Vec<String> {
    // NOTE(ag): It's possible that `resolution-markers` should actually
    // include conflicting marker info. In which case, we should serialize
    // the entire `UniversalMarker` (taking care to still make the PEP 508
    // simplified). At present, we don't include that info. And as a result,
    // this can lead to duplicate markers, since each represents a fork with
    // the same PEP 508 marker but a different conflict marker. We strip the
    // conflict marker, which can leave duplicate PEP 508 markers.
    //
    // So if we did include the conflict marker, then we wouldn't need to do
    // deduplication.
    //
    // Why don't we include conflict markers though? At present, it's just
    // not clear that they are necessary. So by the principle of being
    // conservative, we don't write them. In particular, I believe the original
    // reason for `resolution-markers` is to prevent non-deterministic locking.
    // But it's not clear that that can occur for conflict markers.
    let mut simplified = vec![];
    // Deduplicate without changing order.
    let mut seen = FxHashSet::default();
    for marker in markers {
        let simplified_marker = SimplifiedMarkerTree::new(requires_python, marker.pep508());
        let Some(simplified_string) = simplified_marker.try_to_string() else {
            continue;
        };
        if seen.insert(simplified_string.clone()) {
            simplified.push(simplified_string);
        }
    }
    simplified
}
```

---

_Referenced in [astral-sh/uv#10818](../../astral-sh/uv/pulls/10818.md) on 2025-01-21 16:50_

---

_Closed by @BurntSushi on 2025-01-21 19:04_

---
