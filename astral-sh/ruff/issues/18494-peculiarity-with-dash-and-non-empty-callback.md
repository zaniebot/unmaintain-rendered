```yaml
number: 18494
title: "Peculiarity with Dash and non-empty callback function argument: ANN01"
type: issue
state: closed
author: mmarras
labels:
  - question
assignees: []
created_at: 2025-06-06T12:56:11Z
updated_at: 2025-07-24T12:02:39Z
url: https://github.com/astral-sh/ruff/issues/18494
synced_at: 2026-01-10T11:09:58Z
```

# Peculiarity with Dash and non-empty callback function argument: ANN01

---

_Issue opened by @mmarras on 2025-06-06 12:56_

### Summary

Ok, so this is more of a quirky edge case than really a request.

In dash, each callback function requires at least one argument, so if I don't need one, I just provide `_`, however, ruff asks me to type hint this which arguably looks very odd to me `_ : str`

It bascially says: This function uses a parameter that I'm not going to care about, but btw. it's a string just fyi. Imho negates the intention of  using `_`: suggesting that you don't have to worry about this one.  

```python
@app.callback(
    Output("readme-markdown", "children"),
    Input("readme-markdown", "id"),
)
def load_readme(_: str) -> str:
    """Load the README.md file from the current directory."""
    readme_path = Path(__file__).parent / "README.md"
    if readme_path.exists():
        return readme_path.read_text(encoding="utf-8")
    return "*No README.md found in this folder.*"
```

---

_Renamed from "Peculiarity with Dash and non-empty function arugment: ANN01" to "Peculiarity with Dash and non-empty function argument: ANN01" by @mmarras on 2025-06-06 12:56_

---

_Renamed from "Peculiarity with Dash and non-empty function argument: ANN01" to "Peculiarity with Dash and non-empty callback function argument: ANN01" by @mmarras on 2025-06-06 12:56_

---

_Comment by @ntBre on 2025-06-06 13:09_

I think you should be able to set the [`lint.flake8-annoations.suppress-dummy-args`](https://docs.astral.sh/ruff/settings/#lint_flake8-annotations_suppress-dummy-args) option to `true` to get this behavior! The default is `false`.

---

_Label `question` added by @ntBre on 2025-06-06 13:09_

---

_Closed by @MichaReiser on 2025-07-24 12:02_

---
