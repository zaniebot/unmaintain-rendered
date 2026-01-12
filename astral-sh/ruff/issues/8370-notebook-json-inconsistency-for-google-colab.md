```yaml
number: 8370
title: Notebook JSON inconsistency for Google Colab
type: issue
state: open
author: dhruvmanila
labels:
  - wish
  - notebook
assignees: []
created_at: 2023-10-31T02:22:31Z
updated_at: 2024-02-06T12:12:15Z
url: https://github.com/astral-sh/ruff/issues/8370
synced_at: 2026-01-12T15:54:48Z
```

# Notebook JSON inconsistency for Google Colab

---

_@dhruvmanila_

I'm not sure if this deserves its own issue, but: Would it be possible to use `extend-include = ["*.ipynb"]` and `ruff format` to **only** re-format the Python code in cells, and **not** the ipynb file itself?

My users typically edit notebooks in Google Colab. Google Colab uses two-space indentation in the ipynb file, instead of the typical one-space indentation. `ruff format` currently re-indents with one space. As such, edit history becomes difficult/impossible to track, because effectively every line gets rewritten between the user saving from Google Colab (changing to 2 spaces) and CI running `ruff format` (changing to 1 space).

Google Colab also puts keys like metadata, nbformat, nbformat_minor at the top of the file, but `ruff format` moves these to the bottom. `ruff format` also changes the order of `execution_count`, `outputs`, etc.

I would love to be able to opt out of ipynb formatting, so that only the Python code inside cells is reformatted.

_Originally posted by @jpmckinney in https://github.com/astral-sh/ruff/issues/5188#issuecomment-1785922267_
            

---

_Label `formatter` added by @MichaReiser on 2023-10-31 02:50_

---

_Label `wish` added by @MichaReiser on 2023-12-22 06:14_

---

_Label `formatter` removed by @dhruvmanila on 2024-02-06 12:12_

---

_Label `notebook` added by @dhruvmanila on 2024-02-06 12:12_

---
