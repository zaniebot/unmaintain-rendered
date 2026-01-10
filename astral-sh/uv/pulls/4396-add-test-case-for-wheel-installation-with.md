```yaml
number: 4396
title: Add test case for wheel installation with different path
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/same-path
created_at: 2024-06-18T18:35:59Z
updated_at: 2024-06-19T14:39:56Z
url: https://github.com/astral-sh/uv/pull/4396
synced_at: 2026-01-10T13:54:02Z
```

# Add test case for wheel installation with different path

---

_Pull request opened by @zanieb on 2024-06-18 18:35_

Regression test for #4391 / https://github.com/astral-sh/uv/pull/4393

---

_Label `testing` added by @zanieb on 2024-06-18 18:36_

---

_Comment by @ThiefMaster on 2024-06-18 18:45_

When you say "path" here, do you mean just the wheel's *filename* or the whole absolute path to it?

I don't think the path where the wheel file is located should matter - `pip` does not care about it, regardless of location it shows this message:

> mypkg is already installed with the same version as the provided wheel. Use --force-reinstall to force an installation of the wheel.

---

_Comment by @zanieb on 2024-06-18 18:47_

I think we're just using the full file path. I can add test coverage for just the name too.

---

_Comment by @ThiefMaster on 2024-06-18 18:50_

FWIW one case where I think using the full path would be weird is when you have a cronjob or similar that downloads a "latest" wheel from some place (GH actions build or similar) to a temporary location, which might be a `/tmp/<randomname>/` folder, and then runs `uv pip install /tmp/<randomname>/*.whl`.

In such a case I think the expected behavior would be to reinstall only if the version changed (or if I provide a switch to force reinstalling)...

---

_Comment by @zanieb on 2024-06-18 18:55_

Here's the mentioned test case https://github.com/astral-sh/uv/pull/4398

We don't do what you want yet :)

---

_Review requested from @konstin by @zanieb on 2024-06-18 21:00_

---

_@konstin approved on 2024-06-19 14:04_

---

_Merged by @zanieb on 2024-06-19 14:39_

---

_Closed by @zanieb on 2024-06-19 14:39_

---

_Branch deleted on 2024-06-19 14:39_

---
