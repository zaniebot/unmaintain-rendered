---
number: 951
title: Support registering custom noqa codes
type: issue
state: closed
author: brandon-leapyear
labels:
  - configuration
assignees: []
created_at: 2022-11-29T00:13:55Z
updated_at: 2022-11-29T03:16:19Z
url: https://github.com/astral-sh/ruff/issues/951
synced_at: 2026-01-10T01:22:38Z
---

# Support registering custom noqa codes

---

_Issue opened by @brandon-leapyear on 2022-11-29 00:13_

Until #872 is implemented, we would like to keep using `# noqa: V101` ignores. This is an [undocumented feature](https://github.com/jendrikseipp/vulture/commit/2488be07b115ef4c45c77c6ea8e500a5fc9c595a) in vulture, but we would still prefer using this for the time being. However, turning on M001 would remove these comments. It would be nice to tell the M001 rule what noqa rules third-party tools might use.

---

_Label `enhancement` added by @charliermarsh on 2022-11-29 01:05_

---

_Comment by @charliermarsh on 2022-11-29 01:06_

Makes sense. (We may also want to support this in `--add-noqa` calls.)

---

_Label `configuration` added by @charliermarsh on 2022-11-29 01:06_

---

_Referenced in [astral-sh/ruff#955](../../astral-sh/ruff/pulls/955.md) on 2022-11-29 03:15_

---

_Closed by @charliermarsh on 2022-11-29 03:16_

---
