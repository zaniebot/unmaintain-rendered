```yaml
number: 892
title: Move in-flight tracking to the download level
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unzip
created_at: 2024-01-12T03:34:21Z
updated_at: 2024-01-12T08:52:24Z
url: https://github.com/astral-sh/uv/pull/892
synced_at: 2026-01-10T15:39:02Z
```

# Move in-flight tracking to the download level

---

_Pull request opened by @charliermarsh on 2024-01-12 03:34_

## Summary

Now that `get_or_build_wheel` will often _also_ handle the unzip step, we need to move our per-target locking (`OnceMap`) up a level. Previously, it was only applied to the unzip step, to prevent us from attempting to unzip into the same target concurrently; now, it's applied at the `get_wheel` level, which includes both downloading and unzipping.

## Test Plan

It seems like none of our existing tests catch this -- perhaps because they're too "simple"? You need to run into a situation in which you're doing multiple source distribution builds concurrently (since they'll all try to download `setuptools`):

```
rm -rf foo && virtualenv --clear .venv && cargo run -p puffin-cli -- pip-compile ./scripts/requirements/pydantic.in  --verbose --cache-dir foo
```


---

_Review requested from @konstin by @charliermarsh on 2024-01-12 03:34_

---

_Label `bug` added by @charliermarsh on 2024-01-12 03:34_

---

_@konstin approved on 2024-01-12 08:52_

---

_Merged by @konstin on 2024-01-12 08:52_

---

_Closed by @konstin on 2024-01-12 08:52_

---

_Branch deleted on 2024-01-12 08:52_

---
