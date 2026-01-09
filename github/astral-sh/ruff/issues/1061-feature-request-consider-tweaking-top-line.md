---
number: 1061
title: "[feature-request] Consider tweaking top-line summary for clarity"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-05T14:21:33Z
updated_at: 2022-12-05T21:12:13Z
url: https://github.com/astral-sh/ruff/issues/1061
synced_at: 2026-01-07T13:12:14-06:00
---

# [feature-request] Consider tweaking top-line summary for clarity

---

_Issue opened by @smackesey on 2022-12-05 14:21_

There have been a couple times I've had to do a double take reading the top-line error summary when running `ruff --fix`. First I'd run `ruff` without `--fix`:

```
Found 9 error(s).
```

Then `ruff --fix`:

```
Found 3 error(s) (6 fixed).
```

The difference in reported error count had me briefly wondering if I'd inadvertently changed my configuration. IMO ruff should always report the total number of errors detected pre-fix, then report how many were fixed and how many remain. Maybe:

```
Found 9 error(s) (6 fixed, 3 remaining).
```

---

_Renamed from "Consider tweaking top-line summary for clarity" to "[feature-request] Consider tweaking top-line summary for clarity" by @smackesey on 2022-12-05 14:23_

---

_Comment by @charliermarsh on 2022-12-05 15:08_

(Makes sense, will change.)

---

_Referenced in [astral-sh/ruff#1067](../../astral-sh/ruff/pulls/1067.md) on 2022-12-05 16:57_

---

_Closed by @charliermarsh on 2022-12-05 21:12_

---
