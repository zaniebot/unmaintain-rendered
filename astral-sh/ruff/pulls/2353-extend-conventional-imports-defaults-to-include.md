```yaml
number: 2353
title: Extend conventional imports defaults to include TensorFlow et al
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
assignees: []
merged: true
base: main
head: extend-conventional-imports
created_at: 2023-01-30T13:12:17Z
updated_at: 2023-01-30T16:04:27Z
url: https://github.com/astral-sh/ruff/pull/2353
synced_at: 2026-01-12T04:52:00Z
```

# Extend conventional imports defaults to include TensorFlow et al

---

_Pull request opened by @sbrugman on 2023-01-30 13:12_

Based on configuration from Visual Studio for Python (https://code.visualstudio.com/docs/python/editing#_quick-fixes) and suggestion by @edgarrmondragon.

Semi-common conventions (e.g. `scipy.io` as `spio` or `sio`) are not included.

### Alternatives considered
In principle these can be user configured, but we should have sensible defaults where possible. Criteria to include `tensorflow`, `matplotlib`, `holoviews`, `panel`, `plotly.express`, `polars` and `pyarrow` (measured a part of dash) are (1) the abbreviations are used the official documentation and (2) the packages are reasonably popular by a rule of thumb (e.g. > 1k stars on github). 

- tensorflow: https://www.tensorflow.org/overview
- holoview: https://holoviews.org/getting_started/Introduction.html
- panel: https://panel.holoviz.org/getting_started/build_app.html
- plotly.express: https://dash.plotly.com/duplicate-callback-outputs
- matplotlib: https://github.com/matplotlib/matplotlib/search?q=mpl
- polars: https://www.pola.rs/
- pyarrow: https://arrow.apache.org/docs/python/getstarted.html

(as a santity check one can [search](https://github.com/search?l=Python&q=import+pyspark+as&type=Code) all of github)

---

_Comment by @charliermarsh on 2023-01-30 13:14_

These all look reasonable to me, though maybe I'll leave this open for a day or so to give folks a chance to chime in.

I'll also \cc @edgarrmondragon who worked on this.

---

_@edgarrmondragon reviewed on 2023-01-30 15:05_

I like these additions. Wdyt about adding

```python
import polars as pl  # https://www.pola.rs/
import pyarrow as pa  # https://arrow.apache.org/docs/python/getstarted.html
```
?

---

_Label `rule` added by @charliermarsh on 2023-01-30 16:03_

---

_Renamed from "extend conventional imports defaults" to "Extend conventional imports defaults to include TensorFlow et al" by @charliermarsh on 2023-01-30 16:03_

---

_Merged by @charliermarsh on 2023-01-30 16:04_

---

_Closed by @charliermarsh on 2023-01-30 16:04_

---

_Branch deleted on 2023-01-30 16:04_

---
