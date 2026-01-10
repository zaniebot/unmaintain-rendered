---
number: 10327
title: Building multiple packages in a workspace results in single dist folder irrespective of pyproject.toml location
type: issue
state: closed
author: Dzeneralen
labels:
  - wish
assignees: []
created_at: 2025-01-06T15:34:18Z
updated_at: 2025-01-09T08:37:03Z
url: https://github.com/astral-sh/uv/issues/10327
synced_at: 2026-01-10T01:24:52Z
---

# Building multiple packages in a workspace results in single dist folder irrespective of pyproject.toml location

---

_Issue opened by @Dzeneralen on 2025-01-06 15:34_

Hi, this is a question I was unable to find the answer to. 

I've got a setup like the example below: 

```
├── pyproject.toml
├── packages
│   ├── package-a
│   │   ├── pyproject.toml
│   ├── package-b
│   │   ├── pyproject.toml
```

When I run `uv build --all` or `uv build --package package-a` it succesfully builds, but the wheel files end up in `./dist` and not `./packages/package-a/dist` / `./packages/package-b/dist`. 

Is it possible to make the build command create a separate dist for each package within its own local folder?


---

_Renamed from "Building multiple packages in a workspace places all the resulting wheels in to root dist folder" to "Building multiple packages in a workspace results in single dist folder irrespective of pyproject.toml location" by @Dzeneralen on 2025-01-06 15:36_

---

_Label `question` added by @zanieb on 2025-01-06 20:25_

---

_Assigned to @konstin by @zanieb on 2025-01-06 20:25_

---

_Unassigned @konstin by @konstin on 2025-01-07 11:09_

---

_Label `question` removed by @konstin on 2025-01-07 19:01_

---

_Label `wish` added by @konstin on 2025-01-07 19:01_

---

_Label `qiah` added by @konstin on 2025-01-07 19:01_

---

_Label `qiah` removed by @konstin on 2025-01-07 19:02_

---

_Comment by @konstin on 2025-01-07 19:02_

Currently, this is only possible manually with `--out-dir`.

---

_Comment by @Dzeneralen on 2025-01-09 08:37_

That does not enable having separate dist folders for each package out of the box, but I guess it is not a common issue.

Ended up with a manual approach triggering `uv build --out-dir dist` from the package root instead of the workspace root for invididual packages.

Thank you for the quick response! 👍 

---

_Closed by @Dzeneralen on 2025-01-09 08:37_

---
