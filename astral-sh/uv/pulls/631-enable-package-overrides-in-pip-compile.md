```yaml
number: 631
title: "Enable package overrides in `pip-compile`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/overrides
created_at: 2023-12-13T05:18:09Z
updated_at: 2023-12-13T15:03:40Z
url: https://github.com/astral-sh/uv/pull/631
synced_at: 2026-01-12T16:04:05Z
```

# Enable package overrides in `pip-compile`

---

_@charliermarsh_

## Summary

This PR enables overrides to be passed to `pip-compile` and `pip-install` via a new `--overrides` flag.

When overrides are provided, we effectively replace any requirements that are overridden with the overridden versions. This is applied at all depths of the tree.

The merge semantics are such that we replace _all_ requirements of a package with _all_ requirements from the overrides files. So, for example, if a package declares:

```
foo >= 1.0; python_version < '3.11'
foo < 1.0; python_version >= '3.11'
```

And the user provides an override like:
```
foo >= 2.0
```

Then _both_ of the `foo` requirements in the package will be replaced with the override.

If instead, the user provided an override like:
```
foo >= 2.0; python_version < '3.11'
foo < 3.0; python_version >= '3.11'
```

Then we'd replace _both_ of the original `foo` requirements with both of these overrides. (In technical terms, for each package in the requirements file, we flat-map over its overrides.)

Closes https://github.com/astral-sh/puffin/issues/511.

---

_@konstin approved on 2023-12-13 08:55_

---

_Merged by @charliermarsh on 2023-12-13 15:03_

---

_Closed by @charliermarsh on 2023-12-13 15:03_

---

_Branch deleted on 2023-12-13 15:03_

---
