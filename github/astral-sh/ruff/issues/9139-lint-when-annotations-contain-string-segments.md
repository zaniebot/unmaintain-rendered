---
number: 9139
title: "Lint when `|` annotations contain string segments"
type: issue
state: closed
author: tylerlaprade
labels:
  - rule
assignees: []
created_at: 2023-12-14T18:58:48Z
updated_at: 2023-12-16T21:22:08Z
url: https://github.com/astral-sh/ruff/issues/9139
synced_at: 2026-01-07T13:12:15-06:00
---

# Lint when `|` annotations contain string segments

---

_Issue opened by @tylerlaprade on 2023-12-14 18:58_

<img width="1138" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/4fffbbd1-c71b-427e-819c-3d0f5cd67009">

This is one of the inputs to my function, after quotes were automatically added (and I manually turned `QuerySet['ContractVersion']` into `'QuerySet[ContractVersion]'` due to #9135 and #9136:
```
contract_versions_list: list['ContractVersion']
    | 'QuerySet[ContractVersion]'
    | None = None,
```

When I manually changed it to the following, the tests ran fine again:
```
contract_versions_list: 'list[ContractVersion] | QuerySet[ContractVersion]| None' = None,
```

I'm on Ruff v0.1.8 in a Django 4.2.7 project, running tests with PyTest 7.4.3

---

_Comment by @charliermarsh on 2023-12-14 19:01_

What was the original annotation?

---

_Comment by @charliermarsh on 2023-12-14 19:05_

The issue here is that you changed `QuerySet['ContractVersion']` to `'QuerySet[ContractVersion]'` which isn't valid -- you need to quote the entire binary operation. But I'm just trying to understand, because you applied that change yourself?

---

_Label `question` added by @charliermarsh on 2023-12-14 19:49_

---

_Comment by @tylerlaprade on 2023-12-14 20:30_

Yes, it looks like I introduced this error when trying to manually resolve the warning from Ruff, since I didn't realize it was invalid to quote just part of it. I'm surprised Pyright doesn't complain about an invalid annotation. Is it possible to make Ruff complain about it?

My original annotation had no quotes in it:
```
list[ContractVersion]
    | QuerySet[ContractVersion]
    | None = None,
```

---

_Comment by @charliermarsh on 2023-12-15 00:07_

We could probably lint against that, yeah.

---

_Renamed from "`TCH` `quote-annotations` fixes introduced runtime errors into my tests" to "Lint when `|` annotations contain string segments" by @charliermarsh on 2023-12-15 00:12_

---

_Label `question` removed by @charliermarsh on 2023-12-15 00:12_

---

_Label `rule` added by @charliermarsh on 2023-12-15 00:12_

---

_Comment by @charliermarsh on 2023-12-15 00:12_

Repurposing the issue to lint against `"int" | str` which is invalid.

---

_Referenced in [astral-sh/ruff#9143](../../astral-sh/ruff/pulls/9143.md) on 2023-12-15 01:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-15 01:38_

---

_Closed by @charliermarsh on 2023-12-16 21:22_

---
