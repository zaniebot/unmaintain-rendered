```yaml
number: 305
title: "Add PackageName::as_dist_info_name"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: as-dist-info-name
created_at: 2023-11-03T08:14:14Z
updated_at: 2023-11-03T08:16:45Z
url: https://github.com/astral-sh/uv/pull/305
synced_at: 2026-01-12T16:03:52Z
```

# Add PackageName::as_dist_info_name

---

_@konstin_

From https://packaging.python.org/en/latest/specifications/recording-installed-packages/#recording-installed-packages

> This directory is named as {name}-{version}.dist-info, with name and version fields corresponding to Core metadata specifications. Both fields must be normalized (see Package name normalization and PEP 440 for the definition of normalization for each field respectively), and replace dash (-) characters with underscore (_) characters, so the .dist-info directory always has exactly one dash (-) character in its stem, separating the name and version fields.

Matching this explanation, the dist info names don't get their own type but are an export format from the `PackageName` type.

Follow up to #278 

---

_Comment by @konstin on 2023-11-03 08:14_

I'm going ahead and merge this since it blocks the partial zip implementation

---

_Merged by @konstin on 2023-11-03 08:16_

---

_Closed by @konstin on 2023-11-03 08:16_

---

_Branch deleted on 2023-11-03 08:16_

---
