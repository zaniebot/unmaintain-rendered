```yaml
number: 2052
title: Improve error message layout for wheel-build failures
type: issue
state: open
author: charliermarsh
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-02-28T20:31:31Z
updated_at: 2024-04-12T19:50:14Z
url: https://github.com/astral-sh/uv/issues/2052
synced_at: 2026-01-12T15:58:34Z
```

# Improve error message layout for wheel-build failures

---

_@charliermarsh_

I find pip's error messages here much cleaner:

![Screenshot 2024-02-28 at 3 30 38 PM](https://github.com/astral-sh/uv/assets/1309177/97cbc9a7-014a-4382-bc17-15ec2650d961)

Whereas we have:

![Screenshot 2024-02-28 at 3 30 45 PM](https://github.com/astral-sh/uv/assets/1309177/912e05d7-1c91-4652-a5b1-34869288dc58)

We should add some indentation, clearer sections, the exit code, etc.

---

_Label `help wanted` added by @charliermarsh on 2024-02-28 20:31_

---

_Label `error messages` added by @charliermarsh on 2024-02-28 20:31_

---

_Comment by @konstin on 2024-02-29 09:15_

I prefer the non-indented version, i believe we're using the screen space much better (https://github.com/astral-sh/uv/pull/354). We should add the exit code though.

---

_Comment by @zanieb on 2024-04-12 19:50_

@charliermarsh is this sufficient with @konstin's change?

---
