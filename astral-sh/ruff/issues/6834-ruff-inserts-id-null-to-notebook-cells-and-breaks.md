```yaml
number: 6834
title: "Ruff inserts `\"id\": null` to notebook cells and breaks GitHub notebook viewer"
type: issue
state: closed
author: harupy
labels:
  - bug
assignees: []
created_at: 2023-08-24T02:53:16Z
updated_at: 2023-08-24T13:20:27Z
url: https://github.com/astral-sh/ruff/issues/6834
synced_at: 2026-01-12T15:54:46Z
```

# Ruff inserts `"id": null` to notebook cells and breaks GitHub notebook viewer

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Summary

https://github.com/harupy/mlflow/blob/3f1650db853d2c61ac0cdb0035be91b10f131859/examples/h2o/random_forest.ipynb

<img width="908" alt="image" src="https://github.com/astral-sh/ruff/assets/17039389/2caaf341-f2f5-4c0a-9018-faa472aeaff8">

https://github.com/harupy/mlflow/commit/3f1650db853d2c61ac0cdb0035be91b10f131859: a commit that replicates what ruff does:



Without `"id": null`, the notebook renders fine:
https://github.com/harupy/mlflow/blob/dab36dc4fd6ac6e442bb78c016e7813ced8c0a21/examples/h2o/random_forest.ipynb

<img width="908" alt="image" src="https://github.com/astral-sh/ruff/assets/17039389/1ffe3727-92da-468a-81e4-bb24a6b6847c">


## How to reproduce

```sh
# Notebook without id
> cat a.ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import math\n",
    "import os\n",
    "\n",
    "math.pi"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python (ruff)",
   "language": "python",
   "name": "ruff"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.11.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}

# Run ruff
> cargo run -p ruff_cli -- check --fix --no-cache a.ipynb
    Finished dev [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/ruff check --fix --no-cache a.ipynb`
Found 1 error (1 fixed, 0 remaining).

# Check
> cat a.ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": null,
   "id": null,
   "metadata": {},
   "outputs": [],
   "source": [
    "import math\n",
    "\n",
    "math.pi"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python (ruff)",
   "language": "python",
   "name": "ruff"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.11.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
```

---

_Renamed from "Ruff inserts `id: null` to notebook cells which breaks the GitHub notebook viewer" to "Ruff inserts `"id": null` to notebook cells which breaks the GitHub notebook viewer" by @harupy on 2023-08-24 02:54_

---

_Comment by @harupy on 2023-08-24 02:55_

Can we skip serializing `id` if it's None?

```diff
diff --git a/crates/ruff/src/jupyter/schema.rs b/crates/ruff/src/jupyter/schema.rs
index b6f9ed3c4..17b03e474 100644
--- a/crates/ruff/src/jupyter/schema.rs
+++ b/crates/ruff/src/jupyter/schema.rs
@@ -120,6 +120,7 @@ pub struct RawCell {
     /// Technically, id isn't required (it's not even present) in schema v4.0 through v4.4, but
     /// it's required in v4.5. Main issue is that pycharm creates notebooks without an id
     /// <https://youtrack.jetbrains.com/issue/PY-59438/Jupyter-notebooks-created-with-PyCharm-are-missing-the-id-field-in-cells-in-the-.ipynb-json>
+    #[serde(skip_serializing_if = "Option::is_none")]
     pub id: Option<String>,
     /// Cell-level metadata.
     pub metadata: Value,
@@ -135,6 +136,7 @@ pub struct MarkdownCell {
     /// Technically, id isn't required (it's not even present) in schema v4.0 through v4.4, but
     /// it's required in v4.5. Main issue is that pycharm creates notebooks without an id
     /// <https://youtrack.jetbrains.com/issue/PY-59438/Jupyter-notebooks-created-with-PyCharm-are-missing-the-id-field-in-cells-in-the-.ipynb-json>
+    #[serde(skip_serializing_if = "Option::is_none")]
     pub id: Option<String>,
     /// Cell-level metadata.
     pub metadata: Value,
@@ -150,6 +152,7 @@ pub struct CodeCell {
     /// Technically, id isn't required (it's not even present) in schema v4.0 through v4.4, but
     /// it's required in v4.5. Main issue is that pycharm creates notebooks without an id
     /// <https://youtrack.jetbrains.com/issue/PY-59438/Jupyter-notebooks-created-with-PyCharm-are-missing-the-id-field-in-cells-in-the-.ipynb-json>
+    #[serde(skip_serializing_if = "Option::is_none")]
     pub id: Option<String>,
     /// Cell-level metadata.
     pub metadata: Value,
```

---

_Renamed from "Ruff inserts `"id": null` to notebook cells which breaks the GitHub notebook viewer" to "Ruff inserts `"id": null` to notebook cells which breaks GitHub notebook viewer" by @harupy on 2023-08-24 03:09_

---

_Renamed from "Ruff inserts `"id": null` to notebook cells which breaks GitHub notebook viewer" to "Ruff inserts `"id": null` to notebook cells and breaks GitHub notebook viewer" by @harupy on 2023-08-24 03:09_

---

_Comment by @harupy on 2023-08-24 03:18_

Found this issue while I was working on https://github.com/mlflow/mlflow/pull/9445

---

_Comment by @charliermarsh on 2023-08-24 03:27_

@konstin - Do you know?

---

_Comment by @harupy on 2023-08-24 03:29_

Tried `nbviewer` as well:

https://nbviewer.org/github/harupy/mlflow/blob/3f1650db853d2c61ac0cdb0035be91b10f131859/examples/h2o/random_forest.ipynb

<img width="1198" alt="image" src="https://github.com/astral-sh/ruff/assets/17039389/05d87072-018a-497c-96ba-9bcb113e418f">


---

_Comment by @harupy on 2023-08-24 03:35_

`nbconvert` fails to convert a notebook with `id=null`.

```
> jupyter nbconvert --to html a.ipynb
[NbConvertApp] Converting notebook a.ipynb to html
[NbConvertApp] ERROR | Notebook JSON is invalid: None is not of type 'string'

Failed validating 'type' in code_cell['properties']['id']:

On instance['cells'][0]['id']:
None
[NbConvertApp] ERROR | Notebook is invalid after preprocessor <nbconvert.preprocessors.tagremove.TagRemovePreprocessor object at 0x7f4bb237bf40>
Traceback (most recent call last):
  File "/home/haru/miniconda3/envs/ruff/bin/jupyter-nbconvert", line 8, in <module>
    sys.exit(main())
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/jupyter_core/application.py", line 285, in launch_instance
    return super().launch_instance(argv=argv, **kwargs)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/traitlets/config/application.py", line 1043, in launch_instance
    app.start()
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/nbconvertapp.py", line 410, in start
    self.convert_notebooks()
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/nbconvertapp.py", line 585, in convert_notebooks
    self.convert_single_notebook(notebook_filename)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/nbconvertapp.py", line 551, in convert_single_notebook
    output, resources = self.export_single_notebook(
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/nbconvertapp.py", line 477, in export_single_notebook
    output, resources = self.exporter.from_filename(
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/templateexporter.py", line 389, in from_filename
    return super().from_filename(filename, resources, **kw)  # type:ignore
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/exporter.py", line 201, in from_filename
    return self.from_file(f, resources=resources, **kw)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/templateexporter.py", line 395, in from_file
    return super().from_file(file_stream, resources, **kw)  # type:ignore
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/exporter.py", line 220, in from_file
    return self.from_notebook_node(
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/html.py", line 259, in from_notebook_node
    html, resources = super().from_notebook_node(nb, resources, **kw)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/templateexporter.py", line 411, in from_notebook_node
    nb_copy, resources = super().from_notebook_node(nb, resources, **kw)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/exporter.py", line 154, in from_notebook_node
    nb_copy, resources = self._preprocess(nb_copy, resources)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/exporter.py", line 354, in _preprocess
    self._validate_preprocessor(nbc, preprocessor)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbconvert/exporters/exporter.py", line 321, in _validate_preprocessor
    nbformat.validate(nbc, relax_add_props=True)
  File "/home/haru/miniconda3/envs/ruff/lib/python3.10/site-packages/nbformat/validator.py", line 502, in validate
    raise error
nbformat.validator.NotebookValidationError: None is not of type 'string'

Failed validating 'type' in code_cell['properties']['id']:

On instance['cells'][0]['id']:
None
```

---

_Label `bug` added by @MichaReiser on 2023-08-24 06:18_

---

_Comment by @harupy on 2023-08-24 07:30_

Can I fix this (if this is really a bug)?

---

_Comment by @MichaReiser on 2023-08-24 07:34_

Sure. I would be interested in @konstin's input because he has experience roundtripping JSON. 

---

_Comment by @konstin on 2023-08-24 08:02_

That's an interesting example because according to the spec ([html](https://nbformat.readthedocs.io/en/latest/format_description.html#cell-ids), [RFC](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#required-field), [schema](https://github.com/jupyter/nbformat/blob/a3e787dccbf629c8cbae46f79e74d53ded8b53a9/nbformat/v4/nbformat.v4.5.schema.json#L192)), `id` is required field in every cell in a jupyter notebook 4.5+, i.e. the notebook posted above is technically invalid. From [JEP 62: required field](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#required-field)):

> "The id field in cells would _always_ be **required** for any future nbformat versions (4.5+). In contrast to an optional field, the required field avoids applications having to conditionally check if an id is present or not."

The notebook still works because tools want to be backwards compatible with the <4.5 format and generally don't validate that their input is valid wrt to the specified version. I believe we shouldn't emit invalid notebooks and instead [generate UUIDs](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#updating-older-formats) (or apply one of the other [suggested strategies](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#case-loading-notebook-without-cell-id)) if we see a notebook version ≥4.5. We should still use `#[serde(skip_serializing_if = "Option::is_none")]` for <4.5 notebooks which does not allow `id`. From [JEP 62: Questions](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#questions):

> 5. How should loaders handle notebook loading errors? 
> 
> On notebook load, if an older format update and fill in ids. If an invalid id format for a 4.5+ file, then raise a validation error like we do for other schema errors. We could auto-correct for bad ids if that's deemed appropriate.

This case i think would be only applicable if we want to increase the nbformat version on write (i think we don't want to do that):

> 7. So if nbformat >= 4.5 loads in a pre 4.5 notebook, then a cell ID would be generated and added to each cell? 
> 
> Yes.


---

_Comment by @harupy on 2023-08-24 11:13_

@konstin Thanks for the comment. This notebook was created 4 years ago. Maybe that's the reason why cell ids are missing. 

---

_Comment by @harupy on 2023-08-24 11:22_

Can ruff just fix code, and not touch cell ids?

---

_Comment by @konstin on 2023-08-24 11:31_

> This notebook was created 4 years ago. Maybe that's the reason why cell ids are missing.

It's definitely really easy to miss as a tool author! (i mean, we also missed that)

> Can ruff just fix code, and not touch cell ids?

Not really, i'm afraid. If ruff were to write a notebook with version 4.5 and no cell ids, ruff would produce an invalid notebook with respect to the schema, which we shouldn't do. Downgrading the version on writing is also not an option because the notebook might use other 4.5+ features. The [JEP](https://github.com/jupyter/enhancement-proposals/blob/master/62-cell-id/cell-id.md#required-field) is pretty clear that you just create some ids (with a preference towards UUIDs), so i'd do that.

---

_Comment by @harupy on 2023-08-24 11:58_

The notebook was created with nbformat 4.2 in Python 2:

```
 "metadata": {
  "kernelspec": {
   "display_name": "Python 2",
   "language": "python",
   "name": "python2"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 2
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython2",
   "version": "2.7.15"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
```

https://raw.githubusercontent.com/harupy/mlflow/3f1650db853d2c61ac0cdb0035be91b10f131859/examples/h2o/random_forest.ipynb

---

_Comment by @konstin on 2023-08-24 12:02_

Sorry, i forgot to check the actual notebook and only looked at the `cat a.ipynb`. For the case of random_forest.ipynb, yes, we should definitely use `#[serde(skip_serializing_if = "Option::is_none")]` and omit adding `id` fields.

---

_Comment by @harupy on 2023-08-24 12:12_

Filed #6851

---

_Closed by @konstin on 2023-08-24 13:20_

---
