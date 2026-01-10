```yaml
number: 376
title: Union types are not narrowed down using isinstance
type: issue
state: closed
author: Surasia
labels: []
assignees: []
created_at: 2025-05-14T08:09:21Z
updated_at: 2025-05-14T08:17:19Z
url: https://github.com/astral-sh/ty/issues/376
synced_at: 2026-01-10T02:34:09Z
```

# Union types are not narrowed down using isinstance

---

_Issue opened by @Surasia on 2025-05-14 08:09_

### Summary

Invoked command: `uvx ty check`

Error:
```
warning[possibly-unbound-attribute]: Attribute `vertices` on type `Armature | Camera | Curve | Curves | GreasePencil | GreasePencilv3
 | Lattice | Light | LightProbe | Mesh | MetaBall | PointCloud | Speaker | Volume | None` is possibly unbound
  --> src/operators/spartan_online_operator.py:76:38
   |
74 |         vg = attachment.vertex_groups.new(name=marker.parent_bone)
75 |         if isinstance(attachment.data, Mesh):
76 |             vg.add([v.index for v in attachment.data.vertices], 1.0, "REPLACE")  # pyright: ignore[reportUnknownMemberType]
   |                                      ^^^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default
```

Even though I checked if `attachment.data` is of type `Mesh`, ty doesn't seem to narrow it down to that in the if clause.

### Version

ty 0.0.1-alpha.1

---

_Comment by @MichaReiser on 2025-05-14 08:15_

I think this is another instance of https://github.com/astral-sh/ty/issues/164

---

_Closed by @MichaReiser on 2025-05-14 08:15_

---
