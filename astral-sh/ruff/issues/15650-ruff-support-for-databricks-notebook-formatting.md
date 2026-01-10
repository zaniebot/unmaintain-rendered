```yaml
number: 15650
title: "Ruff support for Databricks notebook formatting - Ignore lines starting with ! or %"
type: issue
state: open
author: vivek-freddy
labels:
  - core
  - notebook
assignees: []
created_at: 2025-01-21T16:05:19Z
updated_at: 2025-08-18T12:04:46Z
url: https://github.com/astral-sh/ruff/issues/15650
synced_at: 2026-01-10T11:09:57Z
```

# Ruff support for Databricks notebook formatting - Ignore lines starting with ! or %

---

_Issue opened by @vivek-freddy on 2025-01-21 16:05_

I have a .py file which runs as notebook 
```
# Databricks notebook source
!pip install -U dspy python-dotenv
!pip install torchmetrics
dbutils.library.restartPython() 
# COMMAND ----------
from tqdm.notebook import tqdm
tqdm.pandas()
import logging
# COMMAND ----------
!pip install matplotlib seaborn
```


The above is python file which runs as notebook where each cell is defined by # COMMAND in databricks. When applying ruff formatting I want to skip the pip install related lines. I tried using noqa but still above is causing warnings. How do I skip checking lines which have ! or % as starting character?

Warnings which I get is

```
Invalid character "\u21" in token
or
SyntaxError: Simple statements must be separated by newlines or semicolons
```

---

_Label `core` added by @MichaReiser on 2025-01-21 17:11_

---

_Comment by @MichaReiser on 2025-01-21 17:11_

Hi. Databrick files aren't supported today and the only "mitigation" for now is to exclude the files from formatting and linting using `exclude`. 

Related issues: 
* https://github.com/astral-sh/ruff/issues/9189
* https://github.com/astral-sh/ruff/issues/3792

---

_Label `notebook` added by @dhruvmanila on 2025-01-22 06:14_

---

_Comment by @wyattscarpenter on 2025-08-05 10:47_

Took me a while to figure this out myself, @vivek-freddy, but I think the intended solution for this is to rewrite the `!pip install -U dspy python-dotenv` lines as `# MAGIC !pip install -U dspy python-dotenv` lines.

---

_Comment by @vivek-freddy on 2025-08-18 12:04_

Thanks @wyattscarpenter 

---
