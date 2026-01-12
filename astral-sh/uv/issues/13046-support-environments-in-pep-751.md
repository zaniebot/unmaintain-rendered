```yaml
number: 13046
title: "Support `environments` in PEP 751"
type: issue
state: open
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2025-04-22T08:22:45Z
updated_at: 2025-04-22T16:00:47Z
url: https://github.com/astral-sh/uv/issues/13046
synced_at: 2026-01-12T16:01:17Z
```

# Support `environments` in PEP 751

---

_@konstin_

PEP 751 has an `environments` field with a list of Environment Markers for which the lock file is considered compatible with. We should support this field both when writing and when reading.

- [ ] When installing from `pylock.toml`, check that the current environment is compatible.
- [ ] When resolving with support environments, write those to the lockfile.
- [ ] When resolving for a specific platform, write an appropriate marker to the platform. (We could e.g. write the inverse of the union of the markers on edges that would lead to a package that is not part of the resolution or a requirement that is incompatible with it)

---

_Label `enhancement` added by @konstin on 2025-04-22 08:22_

---

_Comment by @charliermarsh on 2025-04-22 13:16_

The third thing seems hard. I don't think we propagate the information around the packages we _didn't_ include, when resolving for a specific platform.

---

_Comment by @konstin on 2025-04-22 13:34_

It's tricky because in `requirements.txt`, we only write the packages and verifying compatibility is responsibility of the installer, while (I assume) for `pylock.toml` we want to be able to install without having to look at the metadata of each package. We can't realistically lock the exact PEP 508 environment, it's too specific to be useful for anything outside container workflows, but without knowing all package metadata, we can't which markers are relevant.

Algorithmically, we could do this for the third item: After we built the resolution graph, we traverse the graph and for each package in the resolution, check each of its dependencies. For each requirement where the marker is not disjoint with the current package's marker in the lockfile and where the requirement is not satisfied by the packages in the resolution, record the intersection for the requirement marker and the package's marker. Example: say pytest 8.3.2 is included in the resolution given `python_version >= "3.12"`, and it has a dependencies `colorama>=0.4.6 ; sys_platform == 'win32'`. This resolution happened for linux and no other package required colorama, so `colorama>=0.4.6` is not satisfied, and we record `sys_platform == 'win32' and python_version >= "3.12`. Eventually, we take the union of all those markers: This expressions describes all platforms we don't support, installing on them would lead to a package if unfulfilled requirements. We invert this marker and record it as the supported environment.

I also quickly looked at the second item but it seems we already dropped the environments information from options at the stage of writing the lockfile, so we need to thread it through.

---

_Comment by @charliermarsh on 2025-04-22 14:03_

Yeah we can definitely do it, I just don't think we record the dependencies in this way by the time we get to `ResolverOutput`.

---

_Comment by @konstin on 2025-04-22 16:00_

I'm with you, this needs to happen at an earlier stage where that info still exists.

---
