---
number: 9962
title: "Feature request: Make F401 unused-imports configurable for modules with side effects"
type: issue
state: closed
author: Hnasar
labels:
  - configuration
  - help wanted
assignees: []
created_at: 2024-02-12T20:58:20Z
updated_at: 2024-10-03T19:44:45Z
url: https://github.com/astral-sh/ruff/issues/9962
synced_at: 2026-01-07T13:12:15-06:00
---

# Feature request: Make F401 unused-imports configurable for modules with side effects

---

_Issue opened by @Hnasar on 2024-02-12 20:58_

We have a handful of modules that have import side effects. I know, I know… we should not rely on that (#4056) – but in lieu of fixing everything, would it be possible to configure a list of modules that should never be auto-removed?

I see there's https://docs.astral.sh/ruff/settings/#lint_ignore-init-module-imports but I was hoping there's a chance to avoid this footgun which comes up from time to time by specifying which modules are expected to have import side effects.

---

_Referenced in [astral-sh/ruff#9963](../../astral-sh/ruff/pulls/9963.md) on 2024-02-12 20:58_

---

_Comment by @charliermarsh on 2024-02-12 20:59_

I'm supportive of something like this. I think it's been brought up before.


---

_Label `configuration` added by @charliermarsh on 2024-02-13 03:34_

---

_Comment by @hutch3232 on 2024-06-15 13:34_

Not sure if this is useful, but here is a reproducible example of how I am encountering this issue when using the `pyjanitor` (https://github.com/pyjanitor-devs/pyjanitor) package.

```
pip install pandas ruff pyjanitor
pip freeze
```

<details>

multipledispatch==1.0.0
natsort==8.4.0
numpy==1.26.4
packaging==24.1
pandas==2.2.2
pandas-flavor==0.6.0
pyjanitor==0.27.0
python-dateutil==2.9.0.post0
pytz==2024.1
ruff==0.4.9
scipy==1.13.1
six==1.16.0
tzdata==2024.1
xarray==2024.6.0

</details>

`example.py`
```python
import janitor
import pandas as pd

df = pd.DataFrame({"col": [0.25, 0.75]})
tbl = pd.DataFrame({"lo": [0., 0.5],
                    "hi": [0.5, 1.0]})

df.conditional_join(
    tbl,
    ("col", "lo", ">="),
    ("col", "hi", "<")
)
```

```python
df.conditional_join
# <pandas_flavor.register.register_dataframe_method.<locals>.inner.<locals>.AccessorMethod object at 0x0000021B23F0AE70>
```

```
ruff check
# example.py:1:8: F401 [*] `janitor` imported but unused
# Found 1 error.
# [*] 1 fixable with the `--fix` option.
```

---

_Comment by @zanieb on 2024-06-15 14:18_

It shouldn't be too hard to add a configuration option, if someone is interested.

---

_Label `help wanted` added by @zanieb on 2024-06-15 14:18_

---

_Comment by @JonathanPlasse on 2024-08-15 13:44_

> I'm supportive of something like this. I think it's been brought up before.

- #4250

---

_Comment by @Avasam on 2024-08-19 04:47_

This is something Pycln allows configuring, on top of having  automated side-effect detections https://github.com/astral-sh/ruff/issues/8149

---

_Comment by @MarcSkovMadsen on 2024-08-30 15:09_

+1 because `import hvplot.pandas`, `import hvplot.xarray` and similar imports adds a `.hvplot` api for plotting to a Pandas DataFrame, XArray dataset etc.

For more info see https://github.com/holoviz/hvplot/issues/1399



---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 16:33_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-30 16:38_

---

_Referenced in [astral-sh/ruff#13601](../../astral-sh/ruff/pulls/13601.md) on 2024-10-02 10:59_

---

_Closed by @charliermarsh on 2024-10-03 19:44_

---

_Closed by @charliermarsh on 2024-10-03 19:44_

---
