---
number: 12457
title: I001 warning even after running isort
type: issue
state: closed
author: bje-
labels:
  - question
  - isort
assignees: []
created_at: 2024-07-22T17:50:04Z
updated_at: 2024-07-22T22:25:57Z
url: https://github.com/astral-sh/ruff/issues/12457
synced_at: 2026-01-07T13:12:15-06:00
---

# I001 warning even after running isort

---

_Issue opened by @bje- on 2024-07-22 17:50_

One of my source files produces a warning from `ruff`:

```python
from nemo import configfile, regions
from nemo.generators import (CCGT, CCGT_CCS, CST, OCGT, Biofuel, Black_Coal,
                             CentralReceiver, Coal_CCS, DemandResponse, Hydro,
                             PumpedHydroPump, PumpedHydroTurbine, PV1Axis,
                             Wind, WindOffshore)
from nemo.polygons import (WILDCARD, cst_limit, offshore_wind_limit, pv_limit,
                           wind_limit)
from nemo.storage import PumpedHydroStorage
from nemo.types import UnreachableError
```

Running `isort` over the file does not change these statements in any way, so it seems that `ruff` and `isort` are assuming different `isort` settings. I am not running `isort` with any options nor am I using a settings file.

---

_Comment by @charliermarsh on 2024-07-22 21:32_

Ruff's import sorting rule is designed to be compatible with isort assuming `profile = "black"`, but you're using different isort formatting there. We don't support the variants that wrap like you have it wrapped above. Check out the [FAQ](https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort). There are a few other minor deviations that are documented in there.

---

_Label `question` added by @charliermarsh on 2024-07-22 21:33_

---

_Label `isort` added by @charliermarsh on 2024-07-22 21:33_

---

_Closed by @charliermarsh on 2024-07-22 21:33_

---

_Comment by @bje- on 2024-07-22 21:50_

Thanks, and sorry I missed the FAQ entry!

---

_Comment by @charliermarsh on 2024-07-22 22:25_

No prob!

---
