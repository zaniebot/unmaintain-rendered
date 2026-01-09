---
number: 11141
title: Custom auto-imports
type: issue
state: open
author: MartinBernstorff
labels:
  - rule
assignees: []
created_at: 2024-04-25T06:23:42Z
updated_at: 2024-04-25T08:25:31Z
url: https://github.com/astral-sh/ruff/issues/11141
synced_at: 2026-01-07T13:12:15-06:00
---

# Custom auto-imports

---

_Issue opened by @MartinBernstorff on 2024-04-25 06:23_

I have a handful of imports that are always under the same alias, e.g.:

```
pn -> import plotnine as pn
pl -> import polars as pl
pd -> import pandas as pd
Iter -> from iterpy import Iter
```

I would love if Ruff could detect a missing import and, if it matches any of a set of patterns specified in settings, auto-add the corresponding import.

Is this possible with the current feature set? Or could it be added? Love to hear what you think!

---

_Comment by @bersbersbers on 2024-04-25 07:39_

`ruff --show-settings` shows me

```
linter.flake8_import_conventions.aliases = {
        altair = alt,
        holoviews = hv,
        matplotlib = mpl,
        matplotlib.pyplot = plt,
        networkx = nx,
        numpy = np,
        pandas = pd,
        panel = pn,
        plotly.express = px,
        polars = pl,
        pyarrow = pa,
        seaborn = sns,
        tensorflow = tf,
        tkinter = tk,
}
```

Does that help?

---

_Comment by @MartinBernstorff on 2024-04-25 08:02_

This is pretty nice, thanks! It supports part of, but not all of the workflow. What I would love to be able to do is:

1) Write some code, e.g. `pl.DataFrame({"col1": [1,2,3})`
2) Ruff detects a missing import for `pl`.
3) Because `pl` is in the list of "known imports", it is automatically added to the top of the file as `import polars as pl`

VSCode has some of this functionality, but AFAICT, you cannot add your own rules: https://dev.to/krisplatis/auto-add-missing-imports-on-file-save-in-vs-code-1b89

AFAICT, flake8_import_conventions does not automatically add the import.



---

_Label `rule` added by @AlexWaygood on 2024-04-25 08:25_

---
