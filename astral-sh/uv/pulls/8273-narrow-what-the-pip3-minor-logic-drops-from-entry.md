```yaml
number: 8273
title: "Narrow what the pip3.<minor> logic drops from entry points."
type: pull_request
state: merged
author: thatch
labels:
  - bug
assignees: []
merged: true
base: main
head: numeric-console-script-suffix-fix
created_at: 2024-10-16T22:35:41Z
updated_at: 2024-10-17T01:33:14Z
url: https://github.com/astral-sh/uv/pull/8273
synced_at: 2026-01-10T12:54:06Z
```

# Narrow what the pip3.<minor> logic drops from entry points.

---

_Pull request opened by @thatch on 2024-10-16 22:35_

## Summary

The hack in pip itself only modifies entry points called `pip<number>.<number>` and `easy_install-<number>.<number>`, uv previously dropped too many items including any of the form `foo.<number>`.

Found while trying to install `memray` which somewhat notably does not provide an abi3 wheel, so the installed, suffixed script matches.  At a minimum, this makes the installed files match the `entry_points.txt` more than it did previously, which makes `pickley` happy.

## Test Plan

New test provided for previously-untested code.

---

_Review requested from @charliermarsh by @zanieb on 2024-10-16 22:38_

---

_Review request for @charliermarsh removed by @zanieb on 2024-10-16 22:38_

---

_Review requested from @konstin by @zanieb on 2024-10-16 22:38_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-16 22:38_

---

_@charliermarsh approved on 2024-10-16 22:54_

Thanks, this looks correct to me.

---

_Comment by @charliermarsh on 2024-10-16 22:55_

Ah ok, so `memray3.13` was previously matching. Makes sense.

---

_Label `bug` added by @charliermarsh on 2024-10-16 22:56_

---

_Comment by @thatch on 2024-10-16 23:02_

> Ah ok, so `memray3.13` was previously matching. Makes sense.

Right, `bin/memray3.13` was previously being skipped (and not replaced with anything), but the installed `entry_points.txt` still referenced it.

---

_Merged by @charliermarsh on 2024-10-17 01:33_

---

_Closed by @charliermarsh on 2024-10-17 01:33_

---
