```yaml
number: 10504
title: Support Cross-Language (R and Python) in Jupyter Notebook
type: issue
state: closed
author: partrita
labels:
  - wish
  - notebook
assignees: []
created_at: 2024-03-21T06:41:18Z
updated_at: 2024-03-23T22:49:52Z
url: https://github.com/astral-sh/ruff/issues/10504
synced_at: 2026-01-10T11:09:52Z
```

# Support Cross-Language (R and Python) in Jupyter Notebook

---

_Issue opened by @partrita on 2024-03-21 06:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I mainly use Jupyter Notebook, and occasionally I use R in addition to Python code. My issue is that R code is sometimes mistaken for Python code in Ruff.

When I run Ruff in a folder with multiple Jupyter files, I encounter the following error:
```bash
> ruff --version
ruff 0.0.292
> ruff check .
error: Failed to parse 230726_Normalization.ipynb:cell 21:1:80: EOL while scanning string literal
error: Failed to parse 230828_scVelo_lympho.ipynb:cell 52:1:80: Unexpected token 'leiden_res0_4'
error: Failed to parse 230726_Normalization-Copy1.ipynb:cell 24:1:88: EOL while scanning string literal
error: Failed to parse 230726_FeatureSelection.ipynb:cell 11:1:84: EOL while scanning string literal
error: Failed to parse 230727_QC_draft.ipynb:cell 31:2:17: Unexpected token '='
```

Is there a way to exclude or bypass files written in R?
Thanks in advance.


---

_Comment by @dhruvmanila on 2024-03-21 07:41_

To be clear, your Notebook mainly comprises of Python code but _some_ cells could contain R code - is that what you meant? Can you provide an example Notebook? We currently ignore any Notebooks which isn't Python. This information is available in the Notebook metadata which we use here:

https://github.com/astral-sh/ruff/blob/ebeea51b6a9832516de65af8d2050c4e1b06c92d/crates/ruff_notebook/src/notebook.rs#L380-L387

---

_Label `bug` added by @dhruvmanila on 2024-03-21 07:42_

---

_Label `notebook` added by @dhruvmanila on 2024-03-21 07:42_

---

_Label `bug` removed by @MichaReiser on 2024-03-21 07:55_

---

_Label `needs-info` added by @MichaReiser on 2024-03-21 07:55_

---

_Comment by @partrita on 2024-03-22 00:50_

Yes, That's right. Here is my code.

First, importing `rpy2`

```python
import anndata2ri
import logging

import rpy2.rinterface_lib.callbacks as rcb
import rpy2.robjects as ro

rcb.logger.setLevel(logging.ERROR)
ro.pandas2ri.activate()
anndata2ri.activate()

%load_ext rpy2.ipython
```

Second, make an object by R code like this.  

```R
%%R -i data_mat -o doublet_score -o doublet_class

set.seed(123)
sce = scDblFinder(
    SingleCellExperiment(
        list(counts=data_mat),
    ) 
)
doublet_score = sce$scDblFinder.score
doublet_class = sce$scDblFinder.class
```

Thirds, go back to python code and use an object that I made.

```python
adata.obs["scDblFinder_score"] = doublet_score
adata.obs["scDblFinder_class"] = doublet_class
adata.obs.scDblFinder_class.value_counts()
```

This code works, but when I do ruff check. Here is the error msg.

```bash
230726_QC_WT.ipynb:cell 26:1:34: F821 Undefined name `doublet_score`
230726_QC_WT.ipynb:cell 26:2:34: F821 Undefined name `doublet_class`
```

I found out adding `F821` to ingore list in pyproject.toml is fix that.
But I hope better solutions.
Thanks


---

_Comment by @dhruvmanila on 2024-03-22 11:40_

Thank you for providing the code. I'm assuming by `%%R` you're using the `rpy2` package (https://rpy2.github.io/doc/v3.5.x/html/generated_rst/notebooks.html). I'm a bit confused as to why are there syntax errors in the PR description but not through the code snippet you've provided in the comment. Can you clarify on this part?

I see that the error originates because the variable is defined in the R language cell while it's being used in the Python code cell. Currently, we don't support parsing R language which is why you're seeing the `F821` violations. I do see the use case for it but unfortunately it's not in our current scope.

I'd also recommend to upgrade the Ruff version if possible as you're using `0.0.292` which is a bit old.

---

_Label `needs-info` removed by @dhruvmanila on 2024-03-22 11:40_

---

_Label `wish` added by @dhruvmanila on 2024-03-22 11:40_

---

_Renamed from "[Jupyter notebook] Cross-Language Confusion: R and Python in Jupyter Notebook with Ruff" to "Support Cross-Language (R and Python) in Jupyter Notebook" by @dhruvmanila on 2024-03-22 11:41_

---

_Closed by @partrita on 2024-03-23 22:49_

---
