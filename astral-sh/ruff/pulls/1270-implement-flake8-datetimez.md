```yaml
number: 1270
title: implement flake8-datetimez
type: pull_request
state: merged
author: Yasu-umi
labels: []
assignees: []
merged: true
base: main
head: feature/flake8-datetimez
created_at: 2022-12-17T12:27:35Z
updated_at: 2022-12-20T08:15:53Z
url: https://github.com/astral-sh/ruff/pull/1270
synced_at: 2026-01-12T05:36:31Z
```

# implement flake8-datetimez

---

_Pull request opened by @Yasu-umi on 2022-12-17 12:27_

https://github.com/charliermarsh/ruff/issues/1249
https://github.com/pjknkda/flake8-datetimez

## current status
- [x] DTZ001
- [x] DTZ002
- [x] DTZ003
- [x] DTZ004
- [x] DTZ005
- [x] DTZ006
- [x] DTZ007
  - partially implemented
  - can't pass `strptime('something', 'something').replace(tzinfo=<datetime.tzinfo>)`
  - current comment out impl return check twice
    - `strptime('something', 'something')` is required `%z`
    - `.replace(...)` is required `tzinfo=<datetime.tzinfo>`
- [x] DTZ011
- [x] DTZ012

## discussion
- checker can have state (method chain parent `Call`) ?

---

_Marked ready for review by @Yasu-umi on 2022-12-17 12:40_

---

_Comment by @charliermarsh on 2022-12-18 19:27_

Will review this today! Thank you!

---

_Comment by @charliermarsh on 2022-12-18 23:41_

Thanks for this! I did some refactoring to the checks themselves to use some existing utilities we have to match against module member calls (`dealias_call_path`, `match_call_path`, etc. -- check `plugins.rs` for examples).

I have an idea on how to resolve the last outstanding detail (the `DTZ007` check).

---

_Merged by @charliermarsh on 2022-12-19 03:06_

---

_Closed by @charliermarsh on 2022-12-19 03:06_

---

_Branch deleted on 2022-12-20 08:15_

---
