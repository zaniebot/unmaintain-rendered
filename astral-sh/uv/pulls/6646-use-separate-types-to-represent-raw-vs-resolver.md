```yaml
number: 6646
title: Use separate types to represent raw vs. resolver markers
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pre-interpreter
created_at: 2024-08-26T17:22:02Z
updated_at: 2024-08-26T18:00:22Z
url: https://github.com/astral-sh/uv/pull/6646
synced_at: 2026-01-10T13:09:51Z
```

# Use separate types to represent raw vs. resolver markers

---

_Pull request opened by @charliermarsh on 2024-08-26 17:22_

## Summary

This is similar to https://github.com/astral-sh/uv/pull/6171 but more expansive... _Anywhere_ that we test requirements for platform compatibility, we _need_ to respect the resolver-friendly markers. In fixing the motivating issue (#6621), I also realized that we had a bunch of bugs here around `pip install` with `--python-platform` and `--python-version`, because we always performed our `satisfy` and `Plan` operations on the interpreter's markers, not the adjusted markers!

Closes https://github.com/astral-sh/uv/issues/6621.


---

_Label `bug` added by @charliermarsh on 2024-08-26 17:22_

---

_Merged by @charliermarsh on 2024-08-26 18:00_

---

_Closed by @charliermarsh on 2024-08-26 18:00_

---

_Branch deleted on 2024-08-26 18:00_

---
