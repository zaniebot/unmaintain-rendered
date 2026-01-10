---
number: 9237
title: N813 seems too broad in scope
type: issue
state: open
author: Armavica
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-21T21:31:47Z
updated_at: 2024-01-03T11:20:46Z
url: https://github.com/astral-sh/ruff/issues/9237
synced_at: 2026-01-10T01:22:49Z
---

# N813 seems too broad in scope

---

_Issue opened by @Armavica on 2023-12-21 21:31_

Hi, currently N813 is raised even for cases such as
``` python
import netCDF4 as nc
```
which seem innocuous from the point of view of the rule: https://docs.astral.sh/ruff/rules/camelcase-imported-as-lowercase/

Would it make sense to restrict the applicability of this rule to cases where `name.to_lowercase() == asname.to_lowercase()`? I would be happy to send a PR if that was the case!

---

_Comment by @charliermarsh on 2024-01-02 18:22_

I'm a bit torn as this does match "upstream" (i.e. ,`pep8-naming` does flag with the same error), but I agree that this isn't a very helpful violation in practice.

---

_Label `rule` added by @charliermarsh on 2024-01-02 18:22_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-02 18:22_

---

_Comment by @trag1c on 2024-01-03 11:20_

> this does match "upstream"

For me that means `pep8-naming` also implemented this in a suboptimal way—this violation shouldn't show up for module aliases imo.

---
