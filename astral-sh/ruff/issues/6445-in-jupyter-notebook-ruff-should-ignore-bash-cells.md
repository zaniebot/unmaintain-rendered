```yaml
number: 6445
title: In Jupyter Notebook ruff should ignore bash cells
type: issue
state: closed
author: shaneahmed
labels: []
assignees: []
created_at: 2023-08-09T12:01:25Z
updated_at: 2023-08-09T16:04:03Z
url: https://github.com/astral-sh/ruff/issues/6445
synced_at: 2026-01-12T15:54:46Z
```

# In Jupyter Notebook ruff should ignore bash cells

---

_@shaneahmed_

The command you invoked: `ruff check . --fix` 

Pre-commit yaml available here: https://github.com/TissueImageAnalytics/tiatoolbox/blob/d722f67f4c7584db6a6607544477613dee95aac0/.pre-commit-config.yaml#L75

The current Ruff settings (any relevant sections from your `pyproject.toml`) : https://github.com/TissueImageAnalytics/tiatoolbox/blob/d722f67f4c7584db6a6607544477613dee95aac0/pyproject.toml#L74

The current Ruff version (`ruff --version`) : 0.0.283

Pull request for this bug is available at this link: https://github.com/TissueImageAnalytics/tiatoolbox/pull/689

Issue:
The new version 0.0.283 generates errors for jupyter notebooks such as: `E402 Module level import not at top of file` when the first cell is a bash script which installs the package. This is useful for new users who want to test the jupyter notebooks on online platforms such as Google Colab or Anaconda Cloud.

While testing Jupyter Notebooks, ruff should ignore bash scripts at the top of the cell e.g., in this file https://github.com/TissueImageAnalytics/tiatoolbox/blob/upgrade-ruff-version/examples/01-wsi-reading.ipynb the first cell installs the package on Colab which is not available already. Similarly print statements in this cell should be ignored.



---

_Comment by @dhruvmanila on 2023-08-09 13:51_

Hey, thanks for using Ruff with Jupyter Notebooks during the experimental phase :)

So, to give some context, earlier we would ignore any cells containing any of the IPython escape commands[^1]. This is because a cell could contain both the escape commands and valid Python code but it would fail to parse the escape commands. With the recent release, we've added support for parsing such commands and thus we don't ignore such cells. However, a cell is still ignored if it contains [cell magics] as they're applied on the entire cell.

The reason we need to not ignore cells containing both Python code and escape commands is that otherwise we might raise false positive. For example, if cell 1 contains any escape command and an import statement and cell 2 contains the usage of that module, if we ignore cell 1 we would mark the module usage in cell 2 as `undefined-name`.

We [would recommend to ignore](https://astral.sh/blog/ruff-v0.0.276#jupyter-notebook-support) certain rule codes for Jupyter Notebooks using `per-file-ignores`:

```toml
[tool.ruff]
# Allow imports to appear anywhere in Jupyter Notebooks.
per-file-ignores = { "*.ipynb" = ["E402"] }
```

[cell magics]: https://ipython.readthedocs.io/en/stable/interactive/magics.html#cell-magics

[^1]: `!pip install ...`, `%matplotlib ...`, etc.

---

_Comment by @dhruvmanila on 2023-08-09 13:55_

(closing as intended behavior)

---

_Closed by @dhruvmanila on 2023-08-09 13:55_

---

_Comment by @shaneahmed on 2023-08-09 16:04_

I understand Jupyter notebooks is still experimental but it is very useful. Thanks for this :)

Thanks for the suggestions. I now understand the problem and have modified the notebooks accordingly.

---
