```yaml
number: 8277
title: "Implement `safety` or `pip-audit`"
type: issue
state: open
author: rgeronimi
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-10-27T10:18:44Z
updated_at: 2026-01-06T08:46:59Z
url: https://github.com/astral-sh/ruff/issues/8277
synced_at: 2026-01-10T11:09:50Z
```

# Implement `safety` or `pip-audit`

---

_Issue opened by @rgeronimi on 2023-10-27 10:18_

See
https://pypi.org/project/safety/

Incredible value to my past projects as it monitors automatically python libraries dependencies for known vulnerabilities.

Parts of the project are tied to a for-profit business. If that is an issue, maybe an alternative CVE database can be found (did not research it).

---

_Label `plugin` added by @charliermarsh on 2023-10-27 13:46_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-27 13:46_

---

_Comment by @BrittaMeixner on 2023-11-16 15:19_

We could also use that! That is the only one left we cannot automate in VSCode yet

---

_Comment by @romanzdk on 2024-05-02 11:31_

Safety 3 requires registration so maybe pip-audit might be better alternative (?) - https://pypi.org/project/pip-audit/

---

_Comment by @rgeronimi on 2024-11-05 12:07_

Indeed my recent experience shows also that pip-audit seems much better now :
1. Their database is fully open-source (safety is only open-source on a monthly basis, which is inadequate for critical vulnerabilities, and they might change these terms in the future)
2. It is fully synchronized with the osv database
3. It contains cases (at least on my python environment) that safety does not report
4. Safety has a false positive with jinja2 that they have politely declined to fix :
https://github.com/pallets/jinja/issues/1994
5. Pip-audit has the ambition to become part of the pip toolset

If ruff wants to go forward with vulnerability checks, the pip-audit database might be a much better candidate.
I also think ruff could add value by being much faster (pip-audit is quite slow)

---

_Renamed from "Implement `safety`" to "Implement `safety` or `pip-audit`" by @rgeronimi on 2024-11-05 12:07_

---

_Comment by @rgeronimi on 2026-01-06 08:46_

Any desire to include it? Or is it outside the target scope for ruff? Thx

---
