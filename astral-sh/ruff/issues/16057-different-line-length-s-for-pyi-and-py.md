```yaml
number: 16057
title: "different `line-length`s for `.pyi` and `.py`"
type: issue
state: closed
author: jorenham
labels:
  - configuration
assignees: []
created_at: 2025-02-09T22:51:48Z
updated_at: 2025-02-10T07:40:28Z
url: https://github.com/astral-sh/ruff/issues/16057
synced_at: 2026-01-10T11:09:57Z
```

# different `line-length`s for `.pyi` and `.py`

---

_Issue opened by @jorenham on 2025-02-09 22:51_

### Description

From the style guide for stub files in the official Python typing docs (https://typing.readthedocs.io/en/latest/guides/writing_stubs.html#maximum-line-length):

> Stub files should be limited to 130 characters per line.

I agree with this, and want to use `line-length=130` for all of my `.pyi` stub files (if this seems weirdly long to you, then look through some of the stubs of e.g. [`scipy-stubs`](https://github.com/scipy/scipy-stubs), and you'll see why it is the way it is).
But at the same time, I also want to be able to use the oh-so-familiar `line-length` of `88` for my `.py`.

As far as I'm aware, there is currently no mechanism that allows for configuring the `line-length` for `.py` and `.pyi`. 

For libraries where `.py` and `.pyi` live in separate directories, this can easily be worked around with a `ruff.toml` containing e.g.

```toml
extend = "../../pyproject.toml"
line-length = 130
```

But for libraries such as NumPy, where the `.pyi` and the `.py` are harmoniously living side-by-side, this isn't an option. And I'm having trouble figuring out how I can achieve this "two-sizes-fits-all" configuration.

If I'm missing something; consider this a question. Elsewise, this could be labelled as either a gummy bear or a feature request; choose wisely.

---

_Label `configuration` added by @AlexWaygood on 2025-02-09 22:55_

---

_Comment by @MichaReiser on 2025-02-10 07:40_

Thanks for the nice write up and explaining your use case. This makes sense to me but, unfortunately, isn't supported today. What I think the solution to this is to allow file-level overrides (similar to `per-file-ignores` but for arbitrary settings). 

---

_Closed by @MichaReiser on 2025-02-10 07:40_

---
