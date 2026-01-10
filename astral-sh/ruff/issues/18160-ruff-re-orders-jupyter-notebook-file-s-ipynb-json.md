---
number: 18160
title: "ruff re-orders Jupyter notebook file's (*.ipynb) JSON representation"
type: issue
state: open
author: keen85
labels:
  - wish
  - notebook
assignees: []
created_at: 2025-05-18T12:39:43Z
updated_at: 2025-08-11T14:28:25Z
url: https://github.com/astral-sh/ruff/issues/18160
synced_at: 2026-01-10T01:22:59Z
---

# ruff re-orders Jupyter notebook file's (*.ipynb) JSON representation

---

_Issue opened by @keen85 on 2025-05-18 12:39_

### Summary

# Background
The content of Jupyter notebooks files (*.ipynb) are stored as JSON. Actual content of notebook's code cells are some levels down in the object tree (`$.cells[].source`).

# Situation / Problem
I ran `ruff format some_notebook.ipynb`. I noticed that when ruff finds some code cells that need fixing, apart form the code cells, the whole JSON is reordered. To me it seems as if all keys are ordered **alphabetically**.
In general I like this Idea since it enforces a deterministic JSON representation.

However, I use Azure Synapse to create/edit Jupyter notebooks - and it seems that this "editor" has it's own preference when it comes to the order of the keys in the notebook's JSON representation.

So when using Azure Synapse and ruff, this will lead to a flip-flop behavior where the JSON is reordered constantly making the code diff harder to read.

# Minimal example
## `some_notebook.ipynb` before formatting
```json
{
    "metadata": {
        "kernelspec": {
            "name": "synapse_pyspark",
            "display_name": "python"
        },
        "language_info": {
            "name": "python"
        }
    },
    "cells": [
        {
            "cell_type": "code",
            "metadata": {},
            "outputs": [],
            "source": [
                "# this cell will be fixed by ruff\n",
                "result = 1 +     15\n"
            ],
            "execution_count": null
        }
    ],
    "nbformat_minor": 2,
    "nbformat": 4
}
```
## `some_notebook.ipynb` after formatting
exemplary differences:
- `cell` now comes before `metadata`
- `nbformat_minor` and `nbformat_minor` have switched positions
- `execution_count` moved up
```json
{
    "cells": [
        {
            "cell_type": "code",
            "execution_count": null,
            "metadata": {},
            "outputs": [],
            "source": [
                "# this cell will be fixed by ruff\n",
                "result = 1 + 15"
            ]
        }
    ],
    "metadata": {
        "kernelspec": {
            "display_name": "python",
            "name": "synapse_pyspark"
        },
        "language_info": {
            "name": "python"
        }
    },
    "nbformat": 4,
    "nbformat_minor": 2
}
```

Note: If ruff does not detect that at least one code cell requires fixing, no re-ordering is carried out and the original JSON order is left as-is.

# Feature request
It would be great if there was some configuration that would let users decide if they want a _deterministic_ ipynb file (and re-order it) or if ruff should only alter `$.cells[].source` elements within the ipynb and maintain the original order.

### Version

ruff 0.11.10 (b35bf8ae0 2025-05-15)

---

_Comment by @keen85 on 2025-05-18 12:40_

likely related to https://github.com/astral-sh/ruff/issues/10380

---

_Comment by @MichaReiser on 2025-05-18 16:06_

Yes, Ruff sorts the fields alphabetically. This was added in https://github.com/astral-sh/ruff/pull/5114 I do think it could make sense to try to preserve the field ordering. Although I'm not sure how hard that would be. @dhruvmanila do you remember why we use alphabetic sorting here?

---

_Label `wish` added by @MichaReiser on 2025-05-18 16:06_

---

_Label `notebook` added by @MichaReiser on 2025-05-18 16:06_

---

_Comment by @dhruvmanila on 2025-05-18 16:59_

Yeah, IIRC this was because the `jupyter` command that one would use to open notebook / lab would also do the same and we wanted to avoid large diffs when auto-fixing. Clearly, that isn't the case for other clients. https://github.com/astral-sh/ruff/issues/8370 is related as well.

---

_Comment by @MichaReiser on 2025-08-11 14:28_

The easiest here might be to store `RawNotebookMetadata` or even the entire `RawNotebook` as a `serde_json::Value` and `RawNotebook`(`Metadata`) provides getters to pull individual fields.

The main challenge here is `cells` which we update. It might be worth taking a look at schemars, which is now also a thin wrapper around `Value` https://graham.cool/schemars/migrating/

---
