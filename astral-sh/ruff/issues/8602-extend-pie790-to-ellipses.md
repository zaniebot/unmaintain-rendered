```yaml
number: 8602
title: "Extend `PIE790` to ellipses"
type: issue
state: closed
author: doolio
labels:
  - rule
assignees: []
created_at: 2023-11-10T14:55:20Z
updated_at: 2023-11-13T19:28:18Z
url: https://github.com/astral-sh/ruff/issues/8602
synced_at: 2026-01-10T11:09:50Z
```

# Extend `PIE790` to ellipses

---

_Issue opened by @doolio on 2023-11-10 14:55_

_No description provided._

---

_Comment by @charliermarsh on 2023-11-11 15:12_

That would be reasonable. The other option is to implement a standalone rule for this, as in Pylint: https://github.com/astral-sh/ruff/pull/4726. The blocker we ran into there is that we have an existing rule from flake8-pyi ([`PYI013`](https://docs.astral.sh/ruff/rules/ellipsis-in-non-empty-class-body/)) that enforces this logic _just_ for classes, so we likely need to deprecate that rule.

I don't see a reason not to have `ellipsis` and `pass` in the same rule, though. So we could extend PIE790 (under `--preview` for now), and then mark `PYI013` as deprecated.

---

_Label `rule` added by @charliermarsh on 2023-11-11 15:12_

---

_Renamed from "Should PIE790 be extended to ellipses considering they can be used in the same way as pass" to "Extend `PIE790` to ellipses" by @charliermarsh on 2023-11-11 15:12_

---

_Comment by @Skylion007 on 2023-11-11 15:30_

We would have to make sure `PIE790` applies to `.pyi` files too though as we deprecate `PYI013`.

---

_Comment by @charliermarsh on 2023-11-11 23:11_

@Skylion007 -- Confirmed, it does.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-13 02:07_

---

_Closed by @charliermarsh on 2023-11-13 19:28_

---
