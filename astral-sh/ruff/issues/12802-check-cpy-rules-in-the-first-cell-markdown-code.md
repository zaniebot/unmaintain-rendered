```yaml
number: 12802
title: "Check `CPY` rules in the first cell (markdown, code, or raw cell type)"
type: issue
state: open
author: adamjstewart
labels:
  - notebook
assignees: []
created_at: 2024-08-11T12:05:53Z
updated_at: 2024-08-16T15:47:01Z
url: https://github.com/astral-sh/ruff/issues/12802
synced_at: 2026-01-12T15:54:52Z
```

# Check `CPY` rules in the first cell (markdown, code, or raw cell type)

---

_@adamjstewart_

### Keywords

flake8-copyright, CPY, Jupyter Notebook, .ipynb

### Steps to reproduce

Using the following `pyproject.toml`:
```toml
[tool.ruff]
extend-include = ["*.ipynb"]

[tool.ruff.lint]
extend-select = ["CPY"]
preview = true

[tool.ruff.lint.flake8-copyright]
author = "..."
```
if I run `ruff check` on my repo, all files pass except the Jupyter Notebook files. This is because ruff only checks the first 4096 bytes of the file, whereas the copyright may be in the first cell of the notebook farther below.

### Version

Ruff 0.5.7

### Solution

We would likely need to check more bytes, or allow the total number of bytes to be set on a per-repo or per-file basis. For now, the only workaround is to disable CPY on Jupyter Notebooks.

---

_Label `notebook` added by @AlexWaygood on 2024-08-11 12:35_

---

_Comment by @MichaReiser on 2024-08-11 15:05_

Hmm, the notebook code should run on the concatenated contend of all python cells. Could you share a notebook with us that demonstrates the issue?

---

_Label `needs-mre` added by @MichaReiser on 2024-08-11 15:05_

---

_Comment by @adamjstewart on 2024-08-11 15:18_

Try this one: https://github.com/microsoft/torchgeo/blob/v0.5.2/docs/tutorials/getting_started.ipynb

You'll need:
```toml
[tool.ruff.lint.flake8-copyright]
author = "Microsoft Corporation"
notice-rgx = "Copyright \\(c\\)"
```

---

_Comment by @charliermarsh on 2024-08-11 19:21_

Thanks, I'll try to reproduce.

---

_Comment by @charliermarsh on 2024-08-11 19:29_

Hmm, I ran this locally and got:

```
getting_started.ipynb:cell 4:1:1: CPY001 Missing copyright notice at top of file
```

My `pyproject.toml` looks like:

```toml
[tool.ruff]
extend-include = ["*.ipynb"]

[tool.ruff.lint]
extend-select = ["CPY"]
preview = true

[tool.ruff.lint.flake8-copyright]
author = "Microsoft Corporation"
notice-rgx = "Copyright \\(c\\)"
```

---

_Comment by @dhruvmanila on 2024-08-12 05:26_

@adamjstewart Is this working on your end? Could we mark this issue as resolved?

---

_Comment by @adamjstewart on 2024-08-12 06:29_

This is not working, I see CPY001 even though the notebook contains a copyright. 

---

_Comment by @MichaReiser on 2024-08-12 06:33_

I think the problem here is that Ruff only looks at python cells, but the license is in a markdown cell.

And it seems that @charliermarsh is able to successfully reproduce. The expected result is **no diagnostics**

---

_Comment by @MichaReiser on 2024-08-12 06:35_

To get this to work, I think we would have to special case the rule for notebooks or users have to move the license into a python cell.

---

_Comment by @adamjstewart on 2024-08-12 06:40_

Ah, this makes sense. Not sure how hard special casing the rule would be, but I expect it's more common to find copyrights in markdown cells than in code cells. After all, it isn't code, it's a message to the user. 

---

_Label `needs-mre` removed by @MichaReiser on 2024-08-12 06:41_

---

_Comment by @dhruvmanila on 2024-08-12 07:30_

Oh, I see. I misunderstood Charlie's message, sorry for that.

Yeah, we might have to special case this rule with some additional changes in the `Notebook` struct to access the markdown cells. It would also need to be validated in an editor context because we only ask for Python cells from the client:

https://github.com/astral-sh/ruff/blob/5d85fb75eaf03f62c10472e7362ad2f1c8250e7a/crates/ruff_server/src/server.rs#L335-L344

---

_Renamed from "CPY: no support for Jupyter Notebook files" to "Check `CPY` rules in the first cell (markdown, code, or raw cell type)" by @dhruvmanila on 2024-08-16 15:47_

---
