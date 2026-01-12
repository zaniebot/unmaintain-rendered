```yaml
number: 1520
title: "File level `ty` suppression comments"
type: issue
state: open
author: klonuo
labels:
  - suppression
  - wish
assignees: []
created_at: 2025-11-11T02:05:45Z
updated_at: 2025-11-18T16:10:41Z
url: https://github.com/astral-sh/ty/issues/1520
synced_at: 2026-01-12T15:54:25Z
```

# File level `ty` suppression comments

---

_@klonuo_

### Summary

Hi,

I had a file with typing issue on several locations and thought to make file level comment to suppress it.

I tried to add the directive in the top of the file:
```
# ty: ignore[possibly-missing-attribute]
```
as such approach works for ruff, but it didn't work.

Reading the docs I couldn't find a solution either (besides editing config file).

I then read about `type:ignore` PEP 484 directive and that worked:

```
# type: ignore[possibly-missing-attribute]
```

Now I solved my problem and know the approach, but thought to write that maybe allowing also `ty: ignore` for file level suppression would be intuitive.

### Version

ty 0.0.1-alpha.25

---

_Label `suppression` added by @MichaReiser on 2025-11-11 08:03_

---

_Label `wish` added by @MichaReiser on 2025-11-11 08:03_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:29_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
