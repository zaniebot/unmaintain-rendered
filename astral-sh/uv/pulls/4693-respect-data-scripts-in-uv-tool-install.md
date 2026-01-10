```yaml
number: 4693
title: "Respect data scripts in `uv tool install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/data
created_at: 2024-07-01T13:40:17Z
updated_at: 2024-07-01T16:22:39Z
url: https://github.com/astral-sh/uv/pull/4693
synced_at: 2026-01-10T13:48:28Z
```

# Respect data scripts in `uv tool install`

---

_Pull request opened by @charliermarsh on 2024-07-01 13:40_

## Summary

Packages that provide scripts that _aren't_ Python entrypoints need to respected in `uv tool install`. For example, Ruff ships a script in `ruff-0.5.0.data/scripts`.

Unfortunately, the `.data` directory doesn't exist in the virtual environment at all (it's removed, per the spec, after install). So this PR changes the entry point detection to look at the `RECORD` file, which is the only evidence that the scripts were installed.

Closes https://github.com/astral-sh/uv/issues/4691.

## Test Plan

`cargo run uv tool install ruff` (snapshot tests to-come)


---

_Label `bug` added by @charliermarsh on 2024-07-01 13:40_

---

_Label `preview` added by @charliermarsh on 2024-07-01 13:40_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-01 13:41_

---

_Review requested from @konstin by @charliermarsh on 2024-07-01 13:41_

---

_Comment by @charliermarsh on 2024-07-01 13:41_

I have to identify a small package that does this for CI.

---

_@konstin approved on 2024-07-01 14:23_

---

_Comment by @charliermarsh on 2024-07-01 15:39_

It looks like `pipx` does read the entrypoints, but then _also_ inspects the `dist.files` to find binaries. The upside is not clear to me, maybe it works in the absence of `RECORD`?

---

_Comment by @charliermarsh on 2024-07-01 15:42_

Yeah, I think the benefit there is that it works even for packages that don't have a `RECORD` like `egg-info`. But we don't support installing those anyway...

---

_@zanieb approved on 2024-07-01 16:20_

---

_Merged by @charliermarsh on 2024-07-01 16:22_

---

_Closed by @charliermarsh on 2024-07-01 16:22_

---

_Branch deleted on 2024-07-01 16:22_

---
