---
number: 21300
title: ICN rules miss some conventions from flake8-import-conventions
type: issue
state: open
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-11-06T16:56:41Z
updated_at: 2025-11-10T14:33:16Z
url: https://github.com/astral-sh/ruff/issues/21300
synced_at: 2026-01-07T13:12:16-06:00
---

# ICN rules miss some conventions from flake8-import-conventions

---

_Issue opened by @dscorbett on 2025-11-06 16:56_

### Summary

[`unconventional-import-alias` (ICN001)](https://docs.astral.sh/ruff/rules/unconventional-import-alias/) and possibly [`banned-import-alias` (ICN002)](https://docs.astral.sh/ruff/rules/banned-import-alias/) are missing some conventions enforced by the upstream [flake8-import-conventions](https://github.com/joaopalmeiro/flake8-import-conventions).

For ICN001, `plotly.graph_objects` should be imported as `go` and `statsmodels.api` should be imported as `sm`.

For ICN002, `geopandas` should not be imported as `gpd`. Actually, flake8-import-conventions enforces that it not be aliased at all, but Ruff doesn’t support that. The specific alias that prompted the upstream rule is `gpd`, banning which is the closest Ruff can get.

[Example](https://play.ruff.rs/63025674-955a-482b-b786-23badd5686a1):
```console
$ cat >icn.py <<'# EOF'
import geopandas as gpd
import plotly.graph_objects
import statsmodels.api
# EOF

$ ruff --isolated check icn.py --select ICN001,ICN002
All checks passed!

$ flake8 --select IC icn.py
icn.py:1:1: IC002 geopandas should be imported as `import geopandas`
icn.py:2:1: IC008 plotly.graph_objects should be imported as `import plotly.graph_objects as go`
icn.py:3:1: IC010 statsmodels.api should be imported as `import statsmodels.api as sm`
```

### Version

ruff 0.14.3 (8737a2d5f 2025-10-30)

---

_Label `rule` added by @ntBre on 2025-11-10 14:33_

---

_Referenced in [astral-sh/ruff#21373](../../astral-sh/ruff/pulls/21373.md) on 2025-11-11 04:25_

---
