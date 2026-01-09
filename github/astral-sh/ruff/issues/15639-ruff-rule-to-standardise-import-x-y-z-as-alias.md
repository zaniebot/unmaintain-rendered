---
number: 15639
title: "Ruff rule to standardise `import x.y.z as alias` and `from x.y import z as alias`"
type: issue
state: open
author: epenet
labels:
  - rule
assignees: []
created_at: 2025-01-21T13:35:59Z
updated_at: 2025-01-22T08:56:22Z
url: https://github.com/astral-sh/ruff/issues/15639
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff rule to standardise `import x.y.z as alias` and `from x.y import z as alias`

---

_Issue opened by @epenet on 2025-01-21 13:35_

Hi,

The current Home Assistant code base has a mix of `import x.y.z as alias` and `from x.y import z as alias`

This leads to funny aesthetics, for example:

```python
from homeassistant.helpers import device_registry as dr
import homeassistant.helpers.issue_registry as ir
```

or

```python
from homeassistant.const import CONF_DEVICE_CLASS, CONF_NAME
from homeassistant.core import HomeAssistant
import homeassistant.helpers.config_validation as cv
from homeassistant.helpers.entity_platform import AddEntitiesCallback
from homeassistant.helpers.typing import ConfigType, DiscoveryInfoType
```

It would be nice to have a rule to be able to enforce `from x.y import z as alias`, which will also unlock the grouping

```python
from homeassistant.helpers import device_registry as dr, issue_registry as ir
```

or

```python
from homeassistant.const import CONF_DEVICE_CLASS, CONF_NAME
from homeassistant.core import HomeAssistant
from homeassistant.helpers import config_validation as cv
from homeassistant.helpers.entity_platform import AddEntitiesCallback
from homeassistant.helpers.typing import ConfigType, DiscoveryInfoType
```

Current isort configuration:
```toml
[tool.ruff.lint.isort]
force-sort-within-sections = true
known-first-party = [
    "homeassistant",
]
combine-as-imports = true
split-on-trailing-comma = false
```

---

_Comment by @epenet on 2025-01-21 13:46_

Note: these are linked PRs on HA code base
- https://github.com/home-assistant/core/pull/136146 (merged - fixes some initial violations)
- https://github.com/home-assistant/core/pull/136156 (draft of a pylint plugin which would be better replaced with a ruff rule)
- https://github.com/home-assistant/core/pull/136162 (pending - would fix all violations for `homeassistant` namespace)

---

_Comment by @MichaReiser on 2025-01-21 14:01_

I think that overall makes sense but I'm not sure if it should go into isort or any of the other import-related linters. 

---

_Label `rule` added by @MichaReiser on 2025-01-21 14:01_

---

_Referenced in [home-assistant/core#136156](../../home-assistant/core/pulls/136156.md) on 2025-01-21 15:02_

---

_Comment by @AlexWaygood on 2025-01-21 18:12_

Is this a duplicate of https://github.com/astral-sh/ruff/issues/14070?

---

_Comment by @dhruvmanila on 2025-01-22 05:25_

I think #14070 is different than what's being asked here in that the linked issue is requesting to maintain consistency between `import ...` and `from ...` style imports i.e., strictly use one or the other (IIUC reading the issue description).

---

_Comment by @MichaReiser on 2025-01-22 07:35_

> requesting to maintain consistency between import ... and from ... style imports i.e., strictly use one or the other (IIUC reading the issue description

But isn't this the same as is requested in this issue 😆?

---

_Comment by @dhruvmanila on 2025-01-22 08:03_

I think @epenet might be able to provide more context here. Is the request here strictly about converting a `import x.y as z` to `from x import y as z` or does the conversion from `import x.y` to `from x import y` is also applicable here.

While writing this, I realized that considering both might be more useful and thus this can be marked as duplicate of #14070 

---

_Comment by @epenet on 2025-01-22 08:56_

I was not aware of #14070, and it is definitely related.
However, I think that #14070 is too broad as not all imports are equivalent, and you cannot always migrate in both directions between mixed <=> option 1 <=> option 2.

I'll try to add a comment there but maybe keep this open a little while longer...


---

_Referenced in [astral-sh/ruff#14070](../../astral-sh/ruff/issues/14070.md) on 2025-01-22 09:30_

---
