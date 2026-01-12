```yaml
number: 14270
title: Too long equal-star Python version marker is considered false
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2025-06-26T09:51:24Z
updated_at: 2025-07-01T15:48:50Z
url: https://github.com/astral-sh/uv/issues/14270
synced_at: 2026-01-12T16:01:46Z
```

# Too long equal-star Python version marker is considered false

---

_@konstin_

The following test case with a nonsensical marker currently fails in uv-pep508, as the marker is normalized to false, even though there is a possible match:

```rust
    #[test]
    fn maker_normalization_c() {
        let left_tree = MarkerTree::from_str("python_version == '3.10.0.*'").unwrap();
        let left = left_tree.try_to_string().unwrap();
        let right = "python_full_version >= '3.10' and python_full_version < '3.11";
        assert_eq!(left, right, "{left} != {right}");
    }
```

The marker is nonsensical as it's equivalent to the simpler and correctly working `python_version == '3.10'`.

---

_Label `bug` added by @konstin on 2025-06-26 09:51_

---

_Closed by @konstin on 2025-07-01 15:48_

---
