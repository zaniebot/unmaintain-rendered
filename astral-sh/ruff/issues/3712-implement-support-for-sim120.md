```yaml
number: 3712
title: Implement support for SIM120
type: issue
state: closed
author: MaxG87
labels: []
assignees: []
created_at: 2023-03-24T11:40:12Z
updated_at: 2023-03-24T20:56:48Z
url: https://github.com/astral-sh/ruff/issues/3712
synced_at: 2026-01-10T11:09:46Z
```

# Implement support for SIM120

---

_Issue opened by @MaxG87 on 2023-03-24 11:40_

I am using Ruff 0.0.259.

I noticed that the rule [SIM120](https://github.com/MartinThoma/flake8-simplify#SIM120) from `flake8-simplify` is not yet implemented. While this particular rule might become less relevant over time, I just today dealt with a legacy project which still had that issue.

I think this particular rule would lend itself to be automatically fixable too.

It would be great if it could be considered to implement this rule.

---

_Comment by @JonathanPlasse on 2023-03-24 11:58_

Hi,
Thanks for your report.
This is already implemented as `UP004`.

---

_Comment by @charliermarsh on 2023-03-24 20:56_

That's right -- we support it under `pyupgrade`, rather than under `flake8-simplify`.

---

_Closed by @charliermarsh on 2023-03-24 20:56_

---
