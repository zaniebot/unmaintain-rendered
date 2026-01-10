```yaml
number: 6416
title: Env variables for python downloads
type: pull_request
state: merged
author: blueraft
labels:
  - configuration
assignees: []
merged: true
base: main
head: env-var-python-downloads
created_at: 2024-08-22T09:37:47Z
updated_at: 2024-08-23T14:18:54Z
url: https://github.com/astral-sh/uv/pull/6416
synced_at: 2026-01-10T13:09:51Z
```

# Env variables for python downloads

---

_Pull request opened by @blueraft on 2024-08-22 09:37_

## Summary

Resolves #6406

## Test Plan

```
❯ UV_PYTHON_PREFERENCE=only-managed cargo run -q -- sync --show-settings | rg python_preference
    python_preference: OnlyManaged,
❯ UV_PYTHON_PREFERENCE=system cargo run -q -- sync --show-settings | rg python_preference
    python_preference: System,
❯ UV_NO_PYTHON_DOWNLOADS=1 cargo run -q -- sync --show-settings | rg python_downloads
    python_downloads: Never,
❯ UV_ALLOW_PYTHON_DOWNLOADS=1 cargo run -q -- sync --show-settings | rg python_downloads
    python_downloads: Automatic,
```

---

_Review requested from @zanieb by @konstin on 2024-08-22 09:46_

---

_@konstin approved on 2024-08-22 09:46_

---

_@charliermarsh approved on 2024-08-22 12:52_

Thanks, that's great!

---

_Merged by @charliermarsh on 2024-08-22 12:52_

---

_Closed by @charliermarsh on 2024-08-22 12:52_

---

_Label `configuration` added by @charliermarsh on 2024-08-22 12:52_

---

_Comment by @zanieb on 2024-08-22 13:03_

I think this should match https://docs.astral.sh/uv/reference/settings/#python-downloads instead (we can allow a boolish value too if it's important) but this is adding yet another way to configure the same option which is pretty concerning to me.

---

_Comment by @zanieb on 2024-08-22 13:22_

Reverting in https://github.com/astral-sh/uv/pull/6431 — will follow with what I would have done and we can discuss further if necessary.

---
