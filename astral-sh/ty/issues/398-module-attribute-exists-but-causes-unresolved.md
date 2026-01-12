```yaml
number: 398
title: module attribute exists but causes unresolved-attribute
type: issue
state: closed
author: c-ludo-driva
labels: []
assignees: []
created_at: 2025-05-15T01:48:03Z
updated_at: 2025-05-19T19:17:36Z
url: https://github.com/astral-sh/ty/issues/398
synced_at: 2026-01-12T15:54:23Z
```

# module attribute exists but causes unresolved-attribute

---

_@c-ludo-driva_

### Summary

Hi, 

I think the astral python tooling is fantastic, so I'm excited to use ty. I've run into a few unresolved-attribute errors this morning with the pre-release version. 

If I set up an environment with `uv` and a small python script by running:

```{sh}
#!/bin/bash
uv init
uv add xgboost==1.7.1
echo "import xgboost\nprint(xgboost.callback)">>test.py
cat test.py
uvx ty check
```

I get a warning 

```
error[unresolved-attribute]: Type `<module 'xgboost'>` has no attribute `callback`
 --> test.py:4:7
  |
2 | print(xgboost.__version__)
3 | import xgboost
4 | print(xgboost.callback)
  |       ^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default
```

However, the attribute exists and `uv run test.py` runs fine

```
❯ uv run test.py
<module 'xgboost.callback' from '<path to dir>/.venv/lib/python3.12/site-packages/xgboost/callback.py'>

```




### Version

_No response_

---

_Comment by @carljm on 2025-05-15 02:25_

Thanks for the report! In this case, I believe ty is working as intended. The `xgboost.callback` submodule is not explicitly imported in `xgboost/__init__.py`, therefore accessing it as a submodule without explicitly importing it could be an error. As it happens, other submodules of `xgboost` are imported in `xgboost/__init__.py`, and at least one of those does happen to import the `callback` submodule, so at runtime the `callback` submodule is always attached as an attribute of `xgboost`. But this is quite implicit and may even be accidental, and it's too implicit for ty to model. Pyright issues exactly the same error here.

If `xgboost` wishes to commit to having the `callback` submodule always eagerly imported, it should explicitly import it in `xgboost/__init__.py`. Failing that, the safer practice in your code (which will also satisfy ty) would be to `import xgboost.callback` in your own module, before expecting `xgboost.callback` attribute to exist.

---

_Closed by @carljm on 2025-05-15 02:25_

---

_Comment by @kszlim on 2025-05-19 19:09_

Might be similar to this issue, but is this an expected failure?
```
error[unresolved-attribute]: Type `<module 'plotly.graph_objects'>` has no attribute `Figure`
 --> t.py:3:12
  |
1 | import plotly.graph_objects as go
2 |
3 | def t() -> go.Figure:
  |            ^^^^^^^^^
4 |     return go.Figure()
  |
info: rule `unresolved-attribute` is enabled by default
```
So it seems like plotly lazily imports their modules dynamically via https://github.com/plotly/plotly.py/blob/main/_plotly_utils/importers.py#L4C5-L4C20

Is this a case ty should handle?

---

_Comment by @carljm on 2025-05-19 19:17_

The dynamic stuff is hidden from type checkers by https://github.com/plotly/plotly.py/blob/main/plotly/__init__.py#L38

Which makes this an instance of #133 

---
