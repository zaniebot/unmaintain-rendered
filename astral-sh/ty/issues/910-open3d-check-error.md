---
number: 910
title: open3d check error
type: issue
state: closed
author: hyhmrright
labels:
  - imports
assignees: []
created_at: 2025-07-29T02:08:12Z
updated_at: 2026-01-09T02:03:35Z
url: https://github.com/astral-sh/ty/issues/910
synced_at: 2026-01-10T01:48:23Z
---

# open3d check error

---

_Issue opened by @hyhmrright on 2025-07-29 02:08_

### Summary

```
uvx ty check .\mesh.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.                                   
warning[possibly-unbound-attribute]: Attribute `geometry` on type `<module 'open3d'>` is possibly unbound
  --> mesh.py:21:16
   |
20 |     # 构建新的点云
21 |     down_pcd = o3d.geometry.PointCloud()
   |                ^^^^^^^^^^^^
22 |     down_pcd.points = o3d.utility.Vector3dVector(grouped[['x', 'y', 'z']].values)
   |
info: rule `possibly-unbound-attribute` is enabled by default
```

### Version

_No response_

---

_Label `imports` added by @sharkdp on 2025-07-29 06:39_

---

_Comment by @sharkdp on 2025-07-29 06:41_

Thank you for reporting this.

The `open3d` module imports `geometry` conditionally based on whether a GPU is available or not. It's either defined [in this branch](https://github.com/isl-org/Open3D/blob/1c48fcd897d305a121f5a8db6cabde5dd2d6526e/python/open3d/__init__.py#L70) or [in this branch](https://github.com/isl-org/Open3D/blob/1c48fcd897d305a121f5a8db6cabde5dd2d6526e/python/open3d/__init__.py#L104). Ignoring all the other checks and some `try…except` blocks, a simplified version looks like this:

```py
__DEVICE_API__ = "cpu"

if _pybind_cuda.open3d_core_cuda_device_count() > 0:
    # define `geometry`

    __DEVICE_API__ = "cuda"

if __DEVICE_API__ == "cpu":
    # define `geometry`
```

We can not statically determine whether `_pybind_cuda.open3d_core_cuda_device_count() > 0` is true, so ty doesn't know if that first branch is executed.

And since that first branch could have been executed, ty doesn't know whether `__DEVICE_API__` is `"cpu"` or `"cuda"`. Which is why the truthiness of the `__DEVICE_API__ == "cpu"` check can also not be statically determined. And consequently, we emit a `possibly-unbound-attribute` diagnostic, because we conclude that `geometry` might not be defined.

In *theory* it might be possible to somehow determine that these two branches are mutually exclusive, but I don't know if any type checker can perform a sophisticated control flow analysis like this, which would require evaluating that second branch based *on the assumption* that the first branch is executed or not.

That said, it might be reasonable for us to think about silencing `possibly-unbound-attribute` by default, or silencing it for attribute accesses on module literals specifically. It's easy to imagine that there are similar scenarios in which we can't fully analyze whether an attribute is definitely set or not.

---

_Closed by @carljm on 2026-01-09 02:03_

---
