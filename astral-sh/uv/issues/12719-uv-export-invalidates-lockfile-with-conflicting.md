```yaml
number: 12719
title: "`uv export` invalidates lockfile with conflicting groups"
type: issue
state: closed
author: andyatmiami
labels:
  - bug
assignees: []
created_at: 2025-04-07T15:00:07Z
updated_at: 2025-04-07T19:11:00Z
url: https://github.com/astral-sh/uv/issues/12719
synced_at: 2026-01-10T03:41:47Z
```

# `uv export` invalidates lockfile with conflicting groups

---

_Issue opened by @andyatmiami on 2025-04-07 15:00_

### Summary

See [Discord discussion](https://discord.com/channels/1039017663004942429/1357455063655907529) for additional details

`uv lock; uv export --locked --verbose --group jupyter-datascience-image` fails. Without the `--locked`, the following message is logged.
```
DEBUG Ignoring existing lockfile due to change in conflicting groups: `Conflicts([ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("elyra-preferred")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("elyra-trustyai")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("elyra-preferred")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("trustyai")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("datascience-preferred")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("datascience-trustyai")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("datascience-preferred")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("trustyai")) }}, is_inferred_conflict: false }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("jupyter-datascience-image")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("jupyter-trustyai-image")) }}, is_inferred_conflict: true }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("elyra-preferred")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("jupyter-trustyai-image")) }}, is_inferred_conflict: true }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("elyra-trustyai")) }, ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("jupyter-datascience-image")) }}, is_inferred_conflict: true }, ConflictSet { set: {ConflictItem { package: PackageName("notebooks"), conflict: Group(GroupName("datascience-preferred")) }, [...]
```

For sake of reproducibility - please see this [`pyproject.toml` file](https://github.com/andyatmiami/notebooks/blob/d78541817b7f7bfaa27303437641584e9eb1f7c4/pyproject.toml)

### Platform

macOS 15 arm64

### Version

uv 0.6.11 (Homebrew 2025-03-30)

### Python version

Python 3.11.10

---

_Label `bug` added by @andyatmiami on 2025-04-07 15:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-07 18:05_

---

_Closed by @charliermarsh on 2025-04-07 19:11_

---

_Closed by @charliermarsh on 2025-04-07 19:11_

---
