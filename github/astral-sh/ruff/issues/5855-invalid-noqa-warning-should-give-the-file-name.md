---
number: 5855
title: Invalid noqa warning should give the file name
type: issue
state: closed
author: JanEricNitschke
labels: []
assignees: []
created_at: 2023-07-18T05:51:19Z
updated_at: 2023-07-18T14:08:25Z
url: https://github.com/astral-sh/ruff/issues/5855
synced_at: 2026-01-07T13:12:15-06:00
---

# Invalid noqa warning should give the file name

---

_Issue opened by @JanEricNitschke on 2023-07-18 05:51_

I am running ruff via pre-commit and this over multiple files. I just upgraded to v0.0.278 and got the following warning:

```
warning: Invalid `# noqa` directive on line 867: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 869: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 871: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
warning: Invalid `# noqa` directive on line 873: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
```

Here it would be helpful to also be told the file so that i can have a look and fix this.

---

_Comment by @sobolevn on 2023-07-18 06:27_

Working on it :)

---

_Referenced in [litestar-org/litestar#1998](../../litestar-org/litestar/issues/1998.md) on 2023-07-18 06:40_

---

_Referenced in [astral-sh/ruff#5856](../../astral-sh/ruff/pulls/5856.md) on 2023-07-18 06:47_

---

_Comment by @zanieb on 2023-07-18 13:58_

Please note this is a duplicate of https://github.com/astral-sh/ruff/issues/5729

---

_Closed by @charliermarsh on 2023-07-18 14:08_

---
