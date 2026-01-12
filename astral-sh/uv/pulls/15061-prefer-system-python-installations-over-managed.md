```yaml
number: 15061
title: "Prefer system Python installations over managed ones when `--system` is used"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/system-fix-ii
created_at: 2025-08-04T14:34:06Z
updated_at: 2025-08-05T00:54:04Z
url: https://github.com/astral-sh/uv/pull/15061
synced_at: 2026-01-12T16:11:33Z
```

# Prefer system Python installations over managed ones when `--system` is used

---

_@zanieb_

This fixes a regression from 0.8.0 from https://github.com/astral-sh/uv/pull/7934 and follows https://github.com/astral-sh/uv/pull/15059

The regression is from [this change](https://github.com/astral-sh/uv/pull/7934/files#diff-c7a660ac39628d5e12f388b0cacc7360affa3d7bb21191184d7ee78489675e83), which was made because we'd otherwise (with the other changes in that pull request) _filter out_ managed Python interpreters found in virtual environments.

When `--system` is used we'll convert the default Python preference of `managed` to `system` which avoids things like `uv pip install --system` targeting a managed Python installation.

The basic test is

```
uv python install
uv pip install --system anyio
```

Prior to this change, we'd read a managed interpreter from our managed installation directory and target that. After this change, without #15059, we'd read a managed interpreter from the PATH and target that. Both of those experiences are bad, because the managed interpreters are marked as externally managed. After this change, with #15059, we properly target the system interpreter.

Since we use `system` instead of `only-system`, if there is not a system interpreter we'll still retain our existing behavior and use a managed interpreter. This should limit breakage from the change. Given the source of the regression, we could probably use `only-system` here. I don't feel strongly. I think the main benefit of doing so would be that we'd omit the check for managed installations in error messages when an interpreter cannot be found?

We can't really add test coverage here because the test suite always has externally managed interpreters :)

---

_Converted to draft by @zanieb on 2025-08-04 14:36_

---

_Marked ready for review by @zanieb on 2025-08-04 17:51_

---

_@charliermarsh approved on 2025-08-04 23:38_

---

_Merged by @zanieb on 2025-08-05 00:53_

---

_Closed by @zanieb on 2025-08-05 00:53_

---

_Branch deleted on 2025-08-05 00:54_

---

_Label `bug` added by @zanieb on 2025-08-05 00:54_

---
