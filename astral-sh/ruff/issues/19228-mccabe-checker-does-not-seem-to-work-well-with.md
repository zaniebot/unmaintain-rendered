```yaml
number: 19228
title: McCabe checker does not seem to work well with decorators
type: issue
state: closed
author: travis-leith
labels: []
assignees: []
created_at: 2025-07-09T09:37:26Z
updated_at: 2025-07-09T10:46:14Z
url: https://github.com/astral-sh/ruff/issues/19228
synced_at: 2026-01-10T11:09:59Z
```

# McCabe checker does not seem to work well with decorators

---

_Issue opened by @travis-leith on 2025-07-09 09:37_

### Summary

When building a web app in [shiny](https://shiny.posit.co/py/), one ends up with a function called `server` which itself defines a bunch of internal functions with decorators. See [here](https://github.com/posit-dev/py-shinywidgets/blob/main/examples/superzip/app.py) for a realistic example.

What follows is a minrep, not particularly realistic.

```python
def server(input: Inputs, output: Outputs, session: Session) -> None:
    @render.ui
    def f1() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f2() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f3() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f4() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f5() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f6() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f7() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f8() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f9() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")

    @render.ui
    def f10() -> HTML:  # type: ignore
        return ui.HTML("Hello, world!")
```

The above function leads to 

> `server` is too complex (11 > 10)

My understanding of McCabe complexity is that is a measure of branching. Syntactically, there is no branching in the above code. So something about these decorators causes Ruff to miscount.

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05)

---

_Comment by @MichaReiser on 2025-07-09 10:46_

The decorators aren't relevant. The issue is that the rule as it is defined today aggregates the complexity of all nested functions. See https://github.com/astral-sh/ruff/issues/4384

---

_Closed by @MichaReiser on 2025-07-09 10:46_

---
